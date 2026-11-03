# KTR-0015 — Memory-backed emptyDir can exhaust node memory

- **Upstream:** [kubernetes/kubernetes#119611](https://github.com/kubernetes/kubernetes/issues/119611)
- **Module:** 01 — Pod foundations
- **Concepts:** Pod memory accounting, memory limits, memory-backed emptyDir (tmpfs), node memory pressure, OOM and eviction
- **Evidence:** Root cause and mitigation confirmed in maintainer discussion; reporter added documentation guidance.
- **Status in this project:** Source validated; reproduction plan requires an isolated environment safety review.

## Symptom
A workload mounts a memory-backed `emptyDir` and writes data rapidly. With no effective Pod/container memory limit, multiple Pods can consume large amounts of node memory through tmpfs. The report describes OOM events, failed scheduling, node NotReady, and Pods left in ContainerStatusUnknown or Terminating states.

## Root cause
Memory-backed `emptyDir` uses tmpfs. Without appropriate memory limits, the workload can consume node memory beyond what the scheduler's memory requests alone protect against. The scheduler uses requests for placement; requests are not a hard memory cap.

## Resolution evidence
The issue discussion identifies setting container memory limits as the practical mitigation. The maintainer explicitly endorsed memory limits and noted that Pod-level memory limits are also available. A ResourceQuota can enforce that workloads set limits. The issue was closed after the documentation caution was added and the mitigation was clarified.

## Safe reproduction concept
**Do not run this against a host or shared cluster.** The original failure can crash a node. The lab must be bounded inside a disposable VM or a KIND node container with a strict outer memory cap. We must verify that the node container cannot consume host memory without limit before attempting the workload.

A safe lab design should:
1. Create a disposable single-node environment with a hard memory cap.
2. Deploy a Pod mounting `emptyDir.medium: Memory`.
3. Write a controlled amount of data and observe cgroup/node memory.
4. Repeat only within a pre-set cap and stop well before node exhaustion.
5. Add an appropriate memory limit (and optionally an emptyDir `sizeLimit`) and compare behavior.

This case is not approved for execution until the isolation boundary and cleanup procedure are reviewed.

## Expected evidence
- tmpfs usage grows as the workload writes to the memory-backed volume.
- Node or parent-cgroup memory pressure rises.
- Without effective Pod memory limits, multiple workloads can overcommit node memory.
- With appropriate limits, memory consumption is constrained and failures become attributable to the Pod rather than silently exhausting the node.

## Verification criteria
- Demonstrate the failure mode without affecting the host.
- Confirm the memory limit is enforced.
- Confirm the Pod's resource usage and node memory remain within the configured lab boundary.
- Remove the workload and confirm tmpfs data is released.

## Why it belongs early
A Pod's resource requests, limits, and volume behavior are foundational. This incident teaches that scheduler reservations and runtime-enforced memory limits solve different problems.
