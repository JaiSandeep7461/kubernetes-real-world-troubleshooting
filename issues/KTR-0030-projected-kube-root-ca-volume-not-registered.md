# KTR-0030 — Projected kube-root-ca volume fails because ConfigMap is not registered

- **Source:** [kubernetes/kubernetes#105204](https://github.com/kubernetes/kubernetes/issues/105204)
- **Area:** kubelet / projected volumes / ConfigMap informer cache
- **Evidence:** Issue body and maintainer clarification reviewed.
- **Environment:** Kubernetes 1.22.0–1.22.8 and 1.23.0–1.23.5; later reports on newer versions described similar messages.
- **Symptoms:** Pod startup fails with FailedMount: object namespace/kube-root-ca.crt not registered, even though the ConfigMap exists.
- **Root cause:** The original reproducible regression was fixed in Kubernetes 1.22.9+, 1.23.6+ and 1.24+. Maintainers explicitly warned that later reports with the same error text may be different problems and require a fresh reproducer.
- **Resolution:** Upgrade to a fixed release for the original regression. For newer versions, collect exact version, reproducer and impact rather than assuming the old fix failed.
- **Lab safety:** Safe to test in a disposable cluster with version-pinned Kubernetes releases.
- **Learning path:** projected service-account volumes → ConfigMap informer/cache → kubelet volume setup → version regression analysis.
