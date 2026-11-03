# KTR-0032 — Kubelet reports exit code 137 after container exited normally

- **Source:** [kubernetes/kubernetes#104107](https://github.com/kubernetes/kubernetes/issues/104107)
- **Area:** kubelet / PLEG / container status synchronization / eviction and termination
- **Evidence:** Issue body, accepted triage and later reproducer comments reviewed. Still open in fetched snapshot.
- **Environment:** Original reports around Kubernetes 1.21; later reproducer on GKE Kubernetes 1.31.6 and COS, plus a report on Kubernetes 1.34.3 during kubelet eviction/preemption.
- **Symptoms:** A Pod with restartPolicy Never can remain Terminating or show ContainerStatusUnknown/exit code 137 even though runtime logs and process tracing show it received SIGTERM and exited with a different code.
- **Root-cause evidence:** Reporter traced kubelet status conversion and PLEG event handling but did not identify why the wrong status wins. Later reproducer uses ephemeral-storage eviction and parallel Jobs.
- **Resolution:** No merged fix confirmed in the reviewed issue. A reproducible Job manifest is available in comments.
- **Lab safety:** Use a disposable cluster and strict ephemeral-storage quotas; ensure cleanup and avoid stressing shared nodes.
- **Learning path:** PLEG events → CRI status → kubelet status conversion → eviction → Pod phase and container state.
