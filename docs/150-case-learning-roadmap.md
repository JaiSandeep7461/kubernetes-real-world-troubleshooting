# 150-case learning roadmap

## Objective
Build a set of 150 real-world Kubernetes troubleshooting cases, ordered by prerequisite concepts. Every selected case must have a credible source, enough technical detail to investigate, a feasible reproduction path in a disposable environment, and an evidence-backed resolution or workaround. This is a curated learning set, not a claim to include every Kubernetes issue ever reported.

## Learning order and allocation

| Order | Module | Cases | Concepts and prerequisites |
|---:|---|---:|---|
| 1 | Container and Pod foundations | 20 | OCI image basics; Pod lifecycle; Pending, ContainerCreating, ImagePullBackOff, CrashLoopBackOff; init containers; probes; requests/limits; ephemeral containers; termination |
| 2 | ReplicaSets | 10 | Desired vs actual replicas; selectors and labels; adoption/orphaning; scaling; controller reconciliation |
| 3 | Deployments | 15 | Deployment-to-ReplicaSet ownership; rollout; rollout history/rollback; strategies; progress deadlines; maxSurge/maxUnavailable; paused rollouts |
| 4 | Scheduling and Nodes | 15 | Scheduler; resource fit; taints/tolerations; affinity; topology spread; node pressure; eviction; DaemonSets; priorities and preemption |
| 5 | Services and EndpointSlices | 15 | selectors; ClusterIP; NodePort; LoadBalancer; port/targetPort; readiness and endpoint publication; kube-proxy dataplane |
| 6 | DNS and service discovery | 15 | resolv.conf; search domains/ndots; CoreDNS; kube-dns Service; upstream forwarding; NodeLocal DNSCache; UDP/TCP; DNS debugging |
| 7 | ConfigMaps, Secrets and application configuration | 10 | env injection; mounted updates; immutable objects; secret permissions; projected service-account tokens; configuration drift |
| 8 | Storage and CSI | 15 | PV/PVC; StorageClass; binding modes; dynamic provisioning; attach/detach; mount errors; permissions; expansion; snapshots |
| 9 | CNI and advanced networking | 15 | Pod IPAM; routes; MTU; NetworkPolicy; Calico/Cilium/Flannel; conntrack; iptables/nftables; BGP/VXLAN; Ingress/Gateway |
| 10 | Security and admission | 5 | RBAC; service accounts; Pod Security; admission webhooks; TLS and certificates |
| 11 | Observability and cluster operations | 5 | Events/logs/metrics; resource diagnosis; kubelet/runtime symptoms; upgrades and safe recovery |
|  | **Total** | **150** | |

## Why this order
- Pods are the smallest workload unit and the base for every later controller.
- ReplicaSets explain how Kubernetes maintains Pod count; Deployments add revision and rollout management.
- Scheduling and node behavior explain why Pods remain Pending or are evicted.
- Services select and publish ready backends; DNS makes service discovery usable by applications.
- Configuration, storage, networking, and security add dependencies that are easier to reason about after the workload and service path is understood.
- Observability and operations tie the layers together.

This is a dependency-led order, not a rule that every incident belongs to exactly one component. Each case has one primary module plus secondary dependency tags.

## Acceptance criteria for a case in the 150
1. Original source URL is accessible and the report is not merely a search snippet.
2. The symptom, environment, and relevant versions are recorded; unknowns remain explicitly unknown.
3. It is a real failure mode, configuration trap, or operational failure—not merely a feature request or vague question.
4. A bounded reproduction plan is credible for KIND, a disposable Linux VM, or a low-cost disposable cloud cluster. Cases requiring expensive scale or proprietary infrastructure are marked advanced or excluded.
5. A fix, safe workaround, or diagnostic resolution is known from source evidence or can be derived and independently verified. Open unresolved reports are tracked separately, not counted as solved learning cases.
6. We can state expected failure evidence and post-fix success evidence.
7. Duplicate reports are merged or cross-linked rather than counted as separate cases.

## Record statuses
- Candidate: discovered, not source-validated.
- Source validated: original report and environment checked.
- Root cause confirmed: evidence from maintainers, linked code/PR, test, or reproducer supports the cause.
- Lab-ready: reproduction and remediation plan reviewed.
- Reproduced: failure observed in our stated environment.
- Solved & verified: fix/workaround applied and expected behavior checked.

A closed GitHub issue is not automatically a solved case. A reproducible bug with no known remediation is valuable research but does not qualify as a completed case in the 150-case learning set until its resolution path is established.

## Collection workflow
Research one module at a time, starting with Pod foundations. Keep the master register in `data/issue-register.csv`; use one case file per selected issue under `issues/`. Update source evidence and status before moving on. No labs begin until the first module's records and prerequisites have been reviewed together.

## Current baseline
- 13 earlier records are in the register; some are historical or unresolved and may not qualify for the final 150.
- One additional strongly reproducible service-networking case is documented as KTR-0014.
- The new issue #140984 is a useful open research candidate with a deterministic reproduction claim, but it is not counted among the 150 because a confirmed upstream fix is not yet established.
- No reproduction labs have been started.
