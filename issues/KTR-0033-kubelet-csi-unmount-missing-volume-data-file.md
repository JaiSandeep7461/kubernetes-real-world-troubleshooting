# KTR-0033 — CSI unmount retry loop floods kubelet logs when vol_data.json is missing

- **Source:** [kubernetes/kubernetes#116847](https://github.com/kubernetes/kubernetes/issues/116847)
- **Area:** kubelet / CSI volume teardown / actual-state-of-world cache
- **Evidence:** Issue body reviewed. Closed with triage/not-reproducible and lifecycle/rotten labels; no confirmed root cause.
- **Environment:** CSI volume on a Linux node; reporter observed the issue in a production environment.
- **Symptoms:** Kubelet repeatedly logs UnmountVolume.NewUnmounter failures because the CSI volume's `vol_data.json` file is missing. Log flooding can consume node disk and increase paid log ingestion.
- **Root-cause hypothesis:** Reporter suspected kubelet's actual-state-of-world cache incorrectly retained a volume as mounted after teardown. This was not confirmed.
- **Resolution:** No confirmed upstream fix in the issue. Restarting kubelet was reported as a workaround, not a root-cause fix.
- **Lab safety:** Do not delete kubelet volume metadata on a live node. Revisit only with a disposable VM and mock CSI driver.
- **Learning path:** CSI NodeUnpublish/NodeUnstage → kubelet volume metadata → reconciliation cache → retry behavior and log volume.
