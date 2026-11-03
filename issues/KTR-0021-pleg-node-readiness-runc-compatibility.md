# KTR-0021 — PLEG health failures cause node Ready/NotReady flapping

- **Source:** [kubernetes/kubernetes#45419](https://github.com/kubernetes/kubernetes/issues/45419)
- **Area:** kubelet / PLEG / container runtime / Linux process lifecycle
- **Evidence:** Source and long comment thread reviewed; version correlation reported by multiple operators, but not a single maintainer-confirmed universal root cause.
- **Environment:** Historical Kubernetes 1.6.2, CoreOS on AWS, kernel 4.9.24; later reports involved Ubuntu 18.04/20.04 and runc 1.0.0-rc93 with containerd/Docker variants.
- **Symptoms:** Nodes repeatedly transition to NotReady with `PLEG is not healthy`; kubelet reports PLEG has not been active beyond its threshold.
- **Root-cause evidence:** Multiple operators reported that changing containerd/runc versions (notably avoiding runc 1.0.0-rc93 in affected combinations) stopped the issue. Other reports show version combinations where it did not reproduce. Treat this as a runtime-version compatibility / slow container inspection investigation, not a universal PLEG defect.
- **Resolution:** Reported workarounds include upgrading or downgrading the runtime package to a known-working combination. No universal version prescription.
- **Lab safety:** Do not intentionally destabilize a production node. Reproduce only in a disposable VM with pinned historical runtime versions.
- **Learning path:** kubelet health checks → PLEG → CRI/runtime status → runc/containerd compatibility → node conditions.
