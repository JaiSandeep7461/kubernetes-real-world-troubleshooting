# KTR-0025 — Hung NFS volume operations can block kubelet Pod startup

- **Source:** [kubernetes/kubernetes#31272](https://github.com/kubernetes/kubernetes/issues/31272)
- **Area:** kubelet / volume manager / NFS / Pod termination and startup
- **Evidence:** Original detailed reproduction and maintainer discussion reviewed. A maintainer later stated the original reproduction steps no longer worked; subsequent comments describe stale NFS handles and external storage cleanup ordering.
- **Environment:** Historical Kubernetes 1.2.x and master at the time; NFS volume; later reports included AWS EFS-backed NFS.
- **Symptoms:** A Pod using an unavailable or unmountable NFS volume remains Terminating. Volume teardown can block kubelet volume operations, and newly created Pods can remain ContainerCreating while projected/service-account volumes wait.
- **Root cause:** The original report implicated blocked NFS mount/unmount operations and kubelet volume reconciliation. Later reports also identify stale NFS file handles and storage-provider deletion ordering. These are related symptoms but not proven to be one universal root cause.
- **Resolution:** Restore storage reachability and correct volume lifecycle ordering. PVC/PV protection was also discussed as protection against deleting claims while Pods still use them; it is not a fix for every hung NFS mount.
- **Lab safety:** Do not use a shared NFS server. If revisited, use an isolated VM and disposable NFS export with recovery access.
- **Learning path:** volume lifecycle → kubelet volume manager → mount namespaces → NFS failure modes → CSI teardown.
