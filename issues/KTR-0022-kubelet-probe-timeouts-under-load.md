# KTR-0022 — HTTP probes time out while application responds quickly

- **Source:** [kubernetes/kubernetes#89898](https://github.com/kubernetes/kubernetes/issues/89898)
- **Area:** kubelet / probes / node networking and load
- **Evidence:** Issue body and latest discussion reviewed. Maintainers requested kubelet pprof traces; later reports still describe similar symptoms on Kubernetes 1.31.
- **Environment:** Original report used a cluster with many short-lived Pods; reporter tested direct Pod-IP HTTP requests hundreds of thousands of times and observed responses within 7 ms while kubelet probes timed out. Later report: bare-metal, Kubernetes 1.31.3, Fedora CoreOS 41, cri-dockerd, Canal, Rook/Ceph and roughly 200 Pods.
- **Symptoms:** Readiness/liveness HTTP probes fail with `Client.Timeout exceeded while awaiting headers`, although direct requests to the application are fast and application logs show HTTP 200.
- **Root cause:** Not established in the issue. Suspected kubelet-side contention or probe execution path behavior requires profiling evidence.
- **Resolution:** No confirmed fix recorded in the reviewed discussion.
- **Lab safety:** Safe only as a bounded load experiment; monitor node CPU and kubelet health.
- **Learning path:** probe semantics → kubelet prober → HTTP client timeouts → node contention → pprof and packet-level diagnosis.
