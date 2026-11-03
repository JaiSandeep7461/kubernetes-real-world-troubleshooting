# KTR-0034 — High-concurrency Pod creation causes PVC mount timeouts

- **Source:** [kubernetes/kubernetes#110158](https://github.com/kubernetes/kubernetes/issues/110158)
- **Area:** kubelet / volume attach-mount scalability / CSI and storage
- **Evidence:** Issue body reviewed. Closed as lifecycle/rotten; no confirmed root cause or fix established.
- **Environment:** Kubernetes 1.22.8/1.23.1, Ubuntu 20.04, on-prem Kubespray; 10-node cluster.
- **Symptoms:** PVC mounts time out when hundreds of Pods are created concurrently. Reporter claimed reproduction with 800 replicas mounting the same PVC and observed repeated FailedMount events and delayed Pod startup.
- **Root cause:** Not established. Possible bottlenecks include attach/detach controller throughput, CSI driver limits, API pressure and kubelet volume operation serialization; these remain hypotheses.
- **Resolution:** No verified fix recorded in the issue.
- **Lab safety:** Do not reproduce at 800 replicas on a shared or billable cluster. Start with a small, bounded workload in a disposable environment if this case is revisited.
- **Learning path:** PVC/PV binding → attach/detach controller → CSI controller/node plugins → kubelet volume operation concurrency.
