# Initial issue catalogue

These 13 records are in the master CSV. Each entry links to the original upstream issue. The evidence labels describe what was checked in this first pass; they do not claim all root causes or fixes are confirmed.

| Record | Upstream issue | Area | First-pass finding |
|---|---|---|---|
| KTR-0001 | [kubernetes/kubernetes#56903](https://github.com/kubernetes/kubernetes/issues/56903) | DNS/networking | 5-second DNS delays; maintainer suspected packet loss/timeout; exact drop point unknown. Historical versions. |
| KTR-0002 | [projectcalico/calico#2561](https://github.com/projectcalico/calico/issues/2561) | Calico/BGP | BIRD not ready and BGP not established in a multi-interface VM topology; final cause needs comment review. |
| KTR-0003 | [projectcalico/calico#5712](https://github.com/projectcalico/calico/issues/5712) | CNI/authentication | Unauthorized reading ClusterInformation; token rotation was suspected, not conclusively proven. |
| KTR-0004 | [projectcalico/calico#9433](https://github.com/projectcalico/calico/issues/9433) | VXLAN | Cross-node TCP/DNS fail while ICMP works; missing VTEP information was noted in maintainer diagnostics; final resolution unconfirmed. |
| KTR-0005 | [containerd/containerd#5538](https://github.com/containerd/containerd/issues/5538) | Runtime/filesystem | Image pull cleanup hits EBUSY during unmount; closed not planned, no confirmed fix captured. |
| KTR-0006 | [containerd/containerd#8647](https://github.com/containerd/containerd/issues/8647) | Runtime/task lifecycle | Tasks appear UNKNOWN with PID 0; root cause unresolved in fetched snapshot. |
| KTR-0007 | [kubernetes/ingress-nginx#5401](https://github.com/kubernetes/ingress-nginx/issues/5401) | Admission webhook | API server times out calling webhook; endpoint, routing, TLS, and controller health remain possible causes. |
| KTR-0008 | [kubernetes/ingress-nginx#9932](https://github.com/kubernetes/ingress-nginx/issues/9932) | Service endpoints | Controller reports no active endpoints despite a reachability test; controller cache/reconciliation cause not established. |
| KTR-0009 | [kubernetes/ingress-nginx#7043](https://github.com/kubernetes/ingress-nginx/issues/7043) | Secret/Helm install | Missing admission certificate Secret blocks startup; certificate job/install order and ARM compatibility need investigation. |
| KTR-0010 | [kubernetes/autoscaler#1658](https://github.com/kubernetes/autoscaler/issues/1658) | Autoscaling/storage topology | WaitForFirstConsumer and zone-specific node groups complicate scale-up simulation; historical AWS case. |
| KTR-0011 | [kubernetes/autoscaler#6128](https://github.com/kubernetes/autoscaler/issues/6128) | Autoscaling/AWS | Repeated node-group size reconciliation failure; root cause/fix not confirmed. |
| KTR-0012 | [kubernetes/autoscaler#2045](https://github.com/kubernetes/autoscaler/issues/2045) | Drain/availability | Workload downtime reported during scale-down; operational question, not yet established as an autoscaler defect. |
| KTR-0013 | [flannel-io/flannel#2068](https://github.com/flannel-io/flannel/issues/2068) | Linux kernel/Flannel | Reporter demonstrated networking failure without br_netfilter; closed wontfix; Linux VM preferred for controlled validation. |

## Next validation work
Prioritize comments, linked PRs, and maintainer explanations for KTR-0002, 0003, 0004, 0007–0012. Re-check current release relevance before selecting any reproduction. No labs have been started.
