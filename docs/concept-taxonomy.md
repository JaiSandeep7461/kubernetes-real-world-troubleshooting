# Kubernetes concept taxonomy

This is a learning map, not a rigid sequence. Real incidents often cross several layers; trace dependencies in both directions.

## A. Linux and kernel
Processes/signals; PID and network namespaces; memory and OOM; cgroups; VFS/filesystems; mounts; permissions; systemd; kernel modules; sysctl; conntrack.

## B. Networking fundamentals
Ethernet and ARP; interfaces; routing; TCP/UDP; DNS; MTU and fragmentation; firewalls; iptables/nftables; NAT; packet capture.

## C. Container foundations
OCI images and layers; containerd; CRI; runc; shims; overlayfs; snapshotters; container lifecycle; namespaces and cgroups.

## D. Kubernetes architecture and core
API server; etcd; controllers; scheduler; kubelet; kube-proxy; Pods and lifecycle; probes; events; requests/limits; leases.

## E. Cluster networking
CNI; Calico; Cilium; Flannel; IPAM; BGP; VXLAN/VTEP; Services; EndpointSlice; NetworkPolicy; CoreDNS; Ingress; Gateway API.

## F. Storage
PV/PVC; StorageClass; CSI; provisioning; attach/detach; mount/unmount; topology; snapshots; expansion; filesystem ownership.

## G. Scheduling and autoscaling
Scheduler filters; taints/tolerations; affinity; topology spread; Cluster Autoscaler; Karpenter; HPA/VPA; PDB; drain and eviction.

## H. Security and identity
RBAC; service accounts; projected tokens; token rotation; admission webhooks; TLS; Secrets; Pod Security; image provenance and supply chain.

## I. Observability and reliability
Logs; metrics; traces; Prometheus; Grafana; alerting; SLOs; resource pressure; capacity; incident analysis.

## J. Cloud and production operations
AKS/EKS/GKE; cloud IAM; load balancers; VMSS/ASG; managed control planes; upgrades; HA; backup/DR; multi-cluster operations.

## Cross-layer dependency examples
- Pod sandbox: kubelet → CRI → runtime → CNI ADD → Linux namespaces and interfaces.
- Pod DNS: resolver config → DNS Service/endpoints → CNI dataplane → UDP/TCP → node firewall/conntrack.
- Admission timeout: API server → webhook Service/endpoints → network path → TLS/certificate → controller health.
- PVC Pending: scheduler → PVC/PV binding → CSI provisioner → topology/AZ → autoscaler simulation.
- Node scale-down: autoscaler → cordon/drain → eviction/PDB → graceful termination → cloud node-group termination.
- Container UNKNOWN: kubelet status → CRI → containerd task/shim → PID/process reaping → kernel/systemd.

## Learning rule
Start from the observed symptom. Identify the component that reports it, then trace its dependencies down to the host and outward to cloud services. Do not assume the reporting component is the root cause.