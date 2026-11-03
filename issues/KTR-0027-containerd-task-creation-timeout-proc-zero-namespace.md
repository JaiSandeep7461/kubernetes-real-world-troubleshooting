# KTR-0027 — Container creation timeout surfaces /proc/0 namespace error

- **Source:** [containerd/containerd#9160](https://github.com/containerd/containerd/issues/9160)
- **Area:** containerd CRI / runc / process namespaces / timeout handling
- **Evidence:** Issue body and comments reviewed. A proposed PR #10566 documents the suspected timeout path but was closed without merge.
- **Environment:** containerd issue reports under high I/O pressure; later users reported the error on Kubernetes 1.31 and EKS 1.31.5.
- **Symptoms:** Container creation fails with `lstat /proc/0/ns/ipc: no such file or directory`.
- **Root-cause hypothesis:** Under high IOPS, retrieving sandbox task state may time out; a zero-value state PID (0) is then used in namespace lookup, producing the misleading /proc/0 error. This is described in PR #10566, not confirmed as a shipped fix.
- **Resolution:** No merged fix confirmed from the reviewed PR. Treat the timeout as the meaningful signal, not /proc/0 as the underlying cause.
- **Lab safety:** Avoid artificially saturating disk I/O on shared infrastructure.
- **Learning path:** CRI task creation → containerd state lookup → context deadlines → runc namespace joining.
