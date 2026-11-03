# KTR-0026 — CRI reports container RUNNING after its task has stopped

- **Source:** [containerd/containerd#12678](https://github.com/containerd/containerd/issues/12678)
- **Area:** containerd CRI / task exit events / kubelet termination
- **Evidence:** Detailed issue report and follow-up comments reviewed. Issue was auto-closed as stale; a later report in August 2026 describes similar behavior on a newer K3s/containerd stack.
- **Environment:** Original: containerd 1.7.27, runc 1.2.5, Kubernetes 1.31.2. Later recurrence report: K3s v1.36.3+k3s1 and containerd 2.3.2-k3s2 under high-churn CronJobs.
- **Symptoms:** `ctr tasks ls` reports STOPPED, but `crictl inspect` reports CONTAINER_RUNNING with zero `finishedAt`. Kubelet repeatedly attempts StopContainer and the Pod can remain Terminating.
- **Root-cause evidence:** Discussion suggests the task exit notification may be missed during StopContainer context cancellation. A separate PR #11634 addresses lost exit events during containerd restart/recovery, but the reporter explicitly questioned whether that covers the continuous-operation race described here.
- **Resolution:** No confirmed fix for this exact issue in the reviewed discussion. Do not claim PR #11634 resolves it.
- **Lab safety:** Reproduction requires runtime-level fault injection and should be restricted to a disposable Linux VM.
- **Learning path:** CRI ContainerStatus → containerd task state → shim exit event → StopContainer → kubelet pod worker.
