# Kubernetes Troubleshooting Source Directory

This directory lists public sources for discovering Kubernetes and cloud-native troubleshooting cases. It is a discovery index, not a claim that every source contains only defects or that public trackers contain every production incident.

## 1. Kubernetes core and SIGs

| Source | URL | Notes |
|---|---|---|
| Kubernetes core | https://github.com/kubernetes/kubernetes/issues | Core API, kubelet, scheduler, controllers, node behavior |
| Kubernetes Autoscaler | https://github.com/kubernetes/autoscaler/issues | Cluster Autoscaler and related autoscaling components |
| SIG Network | https://github.com/kubernetes/kubernetes/issues?q=is%3Aissue+label%3Asig%2Fnetwork | SIG label search within Kubernetes core |
| SIG Node | https://github.com/kubernetes/kubernetes/issues?q=is%3Aissue+label%3Asig%2Fnode | SIG label search within Kubernetes core |
| SIG Storage | https://github.com/kubernetes/kubernetes/issues?q=is%3Aissue+label%3Asig%2Fstorage | SIG label search within Kubernetes core |
| SIG Scheduling | https://github.com/kubernetes/kubernetes/issues?q=is%3Aissue+label%3Asig%2Fscheduling | SIG label search within Kubernetes core |
| SIG Apps | https://github.com/kubernetes/kubernetes/issues?q=is%3Aissue+label%3Asig%2Fapps | SIG label search within Kubernetes core |
| Kubernetes Enhancements | https://github.com/kubernetes/enhancements/issues | KEPs, feature design and implementation tracking |
| Kubernetes community | https://github.com/kubernetes/community | Community processes and documentation |
| Issue triage guide | https://www.kubernetes.dev/docs/guide/issue-triage/ | Official issue triage guidance |

## 2. Networking

| Source | URL |
|---|---|
| Calico | https://github.com/projectcalico/calico/issues |
| Cilium | https://github.com/cilium/cilium/issues |
| Flannel | https://github.com/flannel-io/flannel/issues |
| CoreDNS | https://github.com/coredns/coredns/issues |
| Ingress NGINX | https://github.com/kubernetes/ingress-nginx/issues |
| Istio | https://github.com/istio/istio/issues |
| Kubernetes Gateway API | https://github.com/kubernetes-sigs/gateway-api/issues |
| MetalLB | https://github.com/metallb/metallb/issues |

## 3. Container runtime and Linux/container foundations

| Source | URL |
|---|---|
| containerd | https://github.com/containerd/containerd/issues |
| CRI-O | https://github.com/cri-o/cri-o/issues |
| runc | https://github.com/opencontainers/runc/issues |
| BuildKit | https://github.com/moby/buildkit/issues |
| Moby / Docker Engine | https://github.com/moby/moby/issues |
| CNI plugins | https://github.com/containernetworking/plugins/issues |
| CNI specification | https://github.com/containernetworking/cni/issues |

## 4. Storage and CSI

| Source | URL |
|---|---|
| CSI external provisioner | https://github.com/kubernetes-csi/external-provisioner/issues |
| CSI external attacher | https://github.com/kubernetes-csi/external-attacher/issues |
| CSI external resizer | https://github.com/kubernetes-csi/external-resizer/issues |
| CSI external snapshotter | https://github.com/kubernetes-csi/external-snapshotter/issues |
| Longhorn | https://github.com/longhorn/longhorn/issues |
| Rook | https://github.com/rook/rook/issues |
| OpenEBS | https://github.com/openebs/openebs/issues |
| Ceph | https://github.com/ceph/ceph/issues |

## 5. Autoscaling, packaging and GitOps

| Source | URL |
|---|---|
| Karpenter core | https://github.com/kubernetes-sigs/karpenter/issues |
| Karpenter AWS provider | https://github.com/aws/karpenter-provider-aws/issues |
| Helm | https://github.com/helm/helm/issues |
| Argo CD | https://github.com/argoproj/argo-cd/issues |
| Argo Workflows | https://github.com/argoproj/argo-workflows/issues |
| Flux CD | https://github.com/fluxcd/flux2/issues |
| Kustomize | https://github.com/kubernetes-sigs/kustomize/issues |

## 6. Monitoring and observability

| Source | URL |
|---|---|
| Prometheus | https://github.com/prometheus/prometheus/issues |
| Grafana | https://github.com/grafana/grafana/issues |
| Alertmanager | https://github.com/prometheus/alertmanager/issues |
| Loki | https://github.com/grafana/loki/issues |
| Tempo | https://github.com/grafana/tempo/issues |
| OpenTelemetry Collector | https://github.com/open-telemetry/opentelemetry-collector/issues |
| Thanos | https://github.com/thanos-io/thanos/issues |
| Jaeger | https://github.com/jaegertracing/jaeger/issues |

## 7. Managed Kubernetes and cloud integrations

| Source | URL | Scope caveat |
|---|---|---|
| AWS Containers Roadmap | https://github.com/aws/containers-roadmap/issues | Covers ECS, ECR, Fargate and EKS; filter for EKS-specific labels/issues |
| Azure AKS | https://github.com/Azure/AKS/issues | Public AKS issue tracker; not private support cases |
| GKE samples | https://github.com/GoogleCloudPlatform/kubernetes-engine-samples/issues | Samples repository only; not a complete GKE incident tracker |
| AWS Load Balancer Controller | https://github.com/kubernetes-sigs/aws-load-balancer-controller/issues | AWS load-balancer integration |
| Azure cloud provider | https://github.com/kubernetes-sigs/cloud-provider-azure/issues | Azure cloud-provider integration |
| GCP cloud provider | https://github.com/kubernetes/cloud-provider-gcp/issues | GCP cloud-provider integration |

## 8. Community troubleshooting and ecosystem discovery

| Source | URL | Notes |
|---|---|---|
| Kubernetes Discuss | https://discuss.kubernetes.io/ | Community troubleshooting and operational discussions |
| Stack Overflow Kubernetes tag | https://stackoverflow.com/questions/tagged/kubernetes | Questions and answers; includes duplicates and non-bug questions |
| Server Fault Kubernetes tag | https://serverfault.com/questions/tagged/kubernetes | Systems administration and production operations |
| CNCF GitHub organizations/projects | https://github.com/cncf | Discover cloud-native projects and their issue trackers |
| CNCF Landscape | https://landscape.cncf.io/ | Discover additional cloud-native tools and projects |

## Collection rules

1. Preserve the original issue/discussion URL and source repository.
2. Record title, creation/update dates, status, affected versions, environment and symptoms.
3. Inspect comments, maintainer responses, linked pull requests and release notes before assigning a root cause.
4. Separate confirmed defects from support questions, feature requests, documentation issues and user configuration errors.
5. Do not treat a closed issue as proof that a fix shipped.
6. Keep raw source counts separate from curated troubleshooting cases. Counts may include duplicates; curated learning cases should be deduplicated by failure mode.
7. Public trackers do not include private customer support incidents. Managed-cloud GitHub repositories are not complete inventories of EKS, AKS or GKE incidents.
8. Treat the source list as expandable. Add a repository only when its scope and relevance to Kubernetes troubleshooting are clear.

## Project status

- Source directory established.
- No labs are authorized at this stage.
- Current Pod-foundation collection target: 20 validated cases.
- Existing Pod-foundation cases: 6.
- Remaining target: 14.
