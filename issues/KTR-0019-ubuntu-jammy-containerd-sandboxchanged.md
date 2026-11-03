# KTR-0019 — kube-system Pods repeatedly restart with SandboxChanged on Ubuntu 22.04

- **Upstream:** [kubernetes/kubernetes#110177](https://github.com/kubernetes/kubernetes/issues/110177)
- **Module:** 01 — Pod foundations (sandbox/runtime dependency)
- **Concepts:** Pod sandbox, containerd, kubelet, systemd cgroups, pause image, kubeadm
- **Evidence:** Original report includes a reproduction topology; multiple reporters documented successful configuration corrections. Exact cause may vary by runtime/package combination.
- **Status:** Historical configuration case; lab candidate only on a matching disposable VM.

## Symptom
On Ubuntu 22.04 with kubeadm and containerd, static control-plane Pods repeatedly restarted. Events included `SandboxChanged`, followed by API server/etcd unavailability.

## Environment from original report
Ubuntu 22.04, Kubernetes 1.24.0, containerd 1.5.9 package, two network interfaces. This is a historical stack and must not be treated as a current-version recipe.

## Root-cause direction and mitigation evidence
Several reporters found that configuring containerd's systemd cgroup driver (`SystemdCgroup = true`) resolved repeated sandbox restarts. Another reporter also corrected the pause/sandbox image version to match kubeadm's expected image. Because the original thread contains multiple environment-specific reports, treat these as configuration checks rather than claiming one universal root cause.

## Reproduction outline
1. Use a disposable Ubuntu 22.04 VM snapshot, not a production host.
2. Install a version-matched kubeadm/kubelet/containerd stack in an isolated test.
3. Record the containerd cgroup driver, kubelet cgroup driver, sandbox image, and kubeadm expected image.
4. Reproduce only with a deliberately mismatched configuration if the selected package versions permit it.
5. Observe kube-system Pod events, containerd logs, and sandbox recreation.
6. Align cgroup driver configuration and sandbox image; restart services in the documented order.

## Verification
Control-plane static Pods remain stable; `SandboxChanged` events stop recurring; kubelet and containerd agree on cgroup driver; configured sandbox image matches kubeadm's expected version.

## Safety and relevance
Historical, distribution/package-specific case. Do not reproduce on AKS or a shared cluster. Current containerd and Kubernetes defaults may differ; research current versions before selecting this for the final 150.
