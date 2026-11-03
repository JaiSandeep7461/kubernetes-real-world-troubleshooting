# KTR-0016 — Pod remains Terminating when containerd cannot destroy the CNI sandbox

- **Upstream:** [containerd/containerd#8197](https://github.com/containerd/containerd/issues/8197)
- **Related upstream CNI fix:** [containernetworking/cni#1072](https://github.com/containernetworking/cni/pull/1072)
- **containerd integration:** [containerd/containerd#10106](https://github.com/containerd/containerd/pull/10106)
- **Module:** 01 — Pod foundations (runtime lifecycle dependency)
- **Concepts:** Pod sandbox teardown, CRI, containerd, CNI cached result, JSON decoding, deletion lifecycle
- **Evidence:** Source validated; maintainers identified a libcni fix and merged the dependency update into containerd main. Users reported recurrence on some 1.7.x versions, so release-specific status must be checked.
- **Status in this project:** Reproduction candidate; not yet lab-ready.

## Symptom
Pods remain in Terminating because kubelet cannot complete KillPodSandbox. The error reports failure to destroy the sandbox network, including messages such as `cni plugin not initialized` and `unexpected end of JSON input` while decoding the cached CNI result.

## Root-cause direction
The issue discussion points to empty or malformed cached CNI result data and libcni handling. A fix in containernetworking/cni was identified and incorporated into containerd main through PR #10106. The discussion includes reports of recurrence on older containerd 1.7.x builds, so do not assume every affected release is fixed.

## Investigation steps
1. Inspect Pod events for repeated FailedKillPod / KillPodSandbox errors.
2. Inspect containerd logs around the exact sandbox ID.
3. Inspect the configured CNI plugin and cached result paths on the disposable node.
4. Compare containerd/libcni versions against the fix lineage.
5. Test deletion after correcting the runtime/CNI version or cache state.

## Reproduction suitability
This is **not yet approved for a KIND-only lab**. It depends on the node's actual CNI cache and containerd integration, and deliberately corrupting cache state may leave the node network unusable. A disposable Linux VM with snapshot/rollback is the safer target. The reproduction must use a test-only CNI configuration and a clear node rebuild path.

## Verification criteria
- The Pod sandbox is removed without repeated KillPodSandbox errors.
- CNI DEL completes and the sandbox network resources are cleaned.
- The Pod reaches deletion rather than remaining Terminating.
- A second Pod can be created and deleted successfully on the same node.

## Why it belongs early
Pod deletion is part of the Pod lifecycle. Understanding that kubelet relies on CRI and CNI teardown prevents treating every Terminating Pod as an API object or controller problem.
