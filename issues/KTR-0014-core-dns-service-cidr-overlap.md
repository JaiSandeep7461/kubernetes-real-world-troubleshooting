# KTR-0014 — CoreDNS Service ClusterIP unreachable from Pods

- **Upstream:** [coredns/coredns#3704](https://github.com/coredns/coredns/issues/3704)
- **Primary concepts:** Service ClusterIP, Pod CIDR, Service CIDR, DNS
- **Evidence status:** Root cause confirmed; reporter validated the correction.
- **Historical note:** This is a general networking configuration failure mode. Validate behavior on a current Kubernetes version before lab use.

## Symptom
A workload Pod could not reach the CoreDNS Service IP. Querying a specific CoreDNS Pod IP worked, while querying through the Service IP timed out.

## Root cause
The configured Pod network CIDR overlapped the Service network CIDR. This caused routing ambiguity for Service traffic. The CoreDNS process itself was not the root cause.

## Resolution evidence
The reporter recreated the cluster with non-overlapping Pod and Service CIDRs. DNS then worked as expected. See the issue comments for the maintainer diagnosis and reporter confirmation.

## Learning dependencies
1. Pod IP and Pod CIDR
2. Service ClusterIP and Service CIDR
3. Service virtual IP implementation / kube-proxy dataplane
4. DNS Service and CoreDNS Pods
5. Route inspection and packet-level troubleshooting

## Reproduction suitability
**Good candidate for a disposable kubeadm-based Linux VM cluster.** Choose two non-overlapping private CIDR ranges. Intentionally create an overlap only in a disposable cluster, observe the Service reachability failure, then rebuild with non-overlapping ranges. Do not attempt to mutate cluster CIDRs in place or run this experiment on production.

## Expected evidence
- DNS query to CoreDNS Service IP times out.
- Direct query to a CoreDNS Pod IP succeeds.
- Pod CIDR and Service CIDR overlap in cluster configuration.

## Verification
- Recreate the cluster with distinct, non-overlapping Pod and Service CIDRs.
- Confirm the CoreDNS Service ClusterIP is reachable.
- Confirm `nslookup kubernetes.default` succeeds from a test Pod.
- Confirm ordinary ClusterIP Services also work.

## Why this case is in the learning order
This case belongs after Pods, Deployments, ReplicaSets, and basic Services. It demonstrates that a DNS symptom can be caused by the Service network design rather than CoreDNS configuration.
