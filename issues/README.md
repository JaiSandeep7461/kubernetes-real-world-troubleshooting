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


## New Pod-foundation cases (first research pass)

| Record | Upstream issue | Finding | Qualification |
|---|---|---|---|
| KTR-0015 | [kubernetes/kubernetes#119611](https://github.com/kubernetes/kubernetes/issues/119611) | Memory-backed emptyDir can consume node memory; maintainers recommend memory limits and quota enforcement. | Root cause/mitigation supported. Lab requires strict memory isolation; not yet approved to run. |
| KTR-0016 | [containerd/containerd#8197](https://github.com/containerd/containerd/issues/8197) | CNI cached-result decoding can block sandbox teardown and leave Pods Terminating; libcni fix lineage identified. | Release applicability varies; safe reproduction needs disposable Linux VM and rollback. |

### Kept out of the 150 for now
- [kubernetes/kubernetes#72593](https://github.com/kubernetes/kubernetes/issues/72593): ReplicaSet hot-loop when kubelet rejects Pods for unsafe sysctls. PR #130411 proposed a partial cold-loop mitigation but was not merged in the fetched snapshot. Track as an unresolved ReplicaSet candidate, not a solved case.
- [kubernetes/kubernetes#106682](https://github.com/kubernetes/kubernetes/issues/106682): exec liveness-probe errors can be discarded rather than counted as failures. A proposed feature-gated PR is under discussion; behavior and release status need validation. Not counted as a solved case.
- [kubernetes/kubernetes#51835](https://github.com/kubernetes/kubernetes/issues/51835): "Pods stuck Terminating" is a broad umbrella report with multiple unrelated causes; unsuitable as one reproducible case. Use narrowly scoped reports instead.

## Additional Pod-foundation cases (second research pass)

| Record | Upstream issue | Finding | Evidence / caveat |
|---|---|---|---|
| KTR-0017 | [kubernetes/kubernetes#136910](https://github.com/kubernetes/kubernetes/issues/136910) | After kubelet restart, a regular container can fail to restart when a restartable init sidecar with startupProbe is running. | Root cause documented in [merged PR #137146](https://github.com/kubernetes/kubernetes/pull/137146). Verify target release/backport. |
| KTR-0018 | [kubernetes/kubernetes#128544](https://github.com/kubernetes/kubernetes/issues/128544) | Missing imagePullSecret warning spam despite successful image pull. | [PR #138432](https://github.com/kubernetes/kubernetes/pull/138432) merged; warning now tied to actual image pull failure. |
| KTR-0019 | [kubernetes/kubernetes#110177](https://github.com/kubernetes/kubernetes/issues/110177) | kube-system Pods repeatedly restart with SandboxChanged on Ubuntu 22.04/containerd. | Multiple users report success after aligning SystemdCgroup and pause image; treat as environment-specific configuration guidance, not universal confirmed cause. |
| KTR-0020 | [kubernetes/kubernetes#86776](https://github.com/kubernetes/kubernetes/issues/86776) | Container restart fails after OOMKilled; reporter traced it to a changed ConfigMap key and deleted mounted file. | Configuration root cause confirmed by reporter; historical Kubernetes 1.14/Docker environment, not a Kubernetes defect. |

### Pod-foundation coverage checkpoint

The register now contains **20 Pod-foundation research records** (KTR-0015 through KTR-0034): 6 previously documented and 14 added in this research pass. This meets the 20-record discovery target, but it does **not** mean all 20 are confirmed upstream defects or lab-ready.

The 14 new records include confirmed historical fixes, unresolved upstream reports, environment-dependent runtime issues, and lower-confidence candidates. In particular, KTR-0027, KTR-0033 and KTR-0034 remain candidates; KTR-0025 has an original reproduction that maintainers said no longer worked; KTR-0026 remains unresolved despite stale closure. Keep these evidence distinctions when filtering the register.

No labs have started. The next step is a quality gate: re-check the weaker records, confirm release applicability and safety, then choose which cases deserve controlled reproduction. Do not force all 20 into labs just to hit a number.

