# KTR-0023 — Exec probes create significant CPU and system-load overhead

- **Source:** [kubernetes/kubernetes#82440](https://github.com/kubernetes/kubernetes/issues/82440)
- **Area:** kubelet / probes / container runtime / CPU
- **Evidence:** Reproduction steps and recent discussion reviewed; issue remains open and triage-accepted in the fetched snapshot.
- **Environment:** Kubernetes 1.15.3, kubeadm, AWS m5.large, Ubuntu 18.04, Docker 18.09.2, VPC CNI 1.5.3. Later operators reported similar overhead with containerd.
- **Symptoms:** Trivial exec probes such as `cat /tmp/healthy` increase system CPU/load materially as Pod replicas grow. Operators report resource savings after reducing exec-probe use.
- **Root-cause evidence:** Each exec probe requires runtime exec machinery. runc 1.2 reportedly reduces some performance cost by avoiding expensive global VFS locking, but discussion indicates kubelet-side process spawning and probe execution overhead remains a concern.
- **Resolution:** Prefer HTTP/TCP/gRPC probes when they accurately represent health; update runtime versions where applicable. These are mitigations, not a universal Kubernetes fix.
- **Lab safety:** Use a disposable single-node cluster and a small replica count; measure baseline and incremental CPU.
- **Learning path:** probe types → kubelet exec path → CRI ExecSync → runc exec → CPU profiling.
