# KTR-0017 — Kubelet restart prevents regular container restart when a restartable init sidecar is running

- **Upstream:** [kubernetes/kubernetes#136910](https://github.com/kubernetes/kubernetes/issues/136910)
- **Fix:** [kubernetes/kubernetes#137146](https://github.com/kubernetes/kubernetes/pull/137146) — merged 2026-03-19
- **Module:** 01 — Pod foundations
- **Concepts:** Pod lifecycle, restartable init containers/sidecars, startupProbe, kubelet restart, container restart policy
- **Evidence:** Root cause confirmed in merged PR and issue reproduction.
- **Status:** Strong lab candidate; verify release/backport status before selecting exact Kubernetes image.

## Symptom
On Kubernetes 1.35.0, a Pod with a restartable init-container sidecar and a regular application container could remain in Error after the application container crashed. The sidecar continued running, while the regular container's restart count remained zero. Restarting kubelet was part of the reproducer.

## Reproduction evidence
The reporter confirmed the behavior after a kubelet restart and then force-exiting the main container with `kill 1`. They built a kubelet from a proposed commit and confirmed that it restarted the application container correctly.

## Root cause
After kubelet restart, the startup-probe state for a running restartable init container with a startupProbe was not seeded as expected when the relevant feature gate was disabled. The init-container action logic did not mark the Pod initialized, so the regular-container restart action was skipped.

## Resolution
PR #137146 merged a fix. The PR description explains the startup-probe state handling correction. Check the target release and backport before using a lab image.

## Lab outline
1. Use a disposable single-node cluster on an affected Kubernetes 1.35 build.
2. Create a Pod with a restartable init container (`restartPolicy: Always`) and a startupProbe, plus a regular app container.
3. Wait until both are running.
4. Restart kubelet on the node.
5. Kill PID 1 in the regular app container.
6. Observe whether kubelet restarts the regular container and updates restartCount.
7. Repeat with a build containing the fix.

## Verification
The regular container restarts after kubelet restart; restartCount increments; Pod conditions recover; the sidecar remains running.

## Safety and scope
Requires a disposable Linux VM or a local cluster where kubelet restart is supported. KIND may be suitable only if kubelet restart can be safely controlled inside the node container. Do not use a shared or production cluster.
