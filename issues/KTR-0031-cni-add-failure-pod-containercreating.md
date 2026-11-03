# KTR-0031 — Failed first CNI ADD leaves Pod stuck in ContainerCreating

- **Source:** [kubernetes/kubernetes#96200](https://github.com/kubernetes/kubernetes/issues/96200)
- **Area:** Pod sandbox creation / CNI ADD-DEL / Multus / Whereabouts IPAM
- **Evidence:** Reproduction description and CNI maintainer discussion reviewed.
- **Environment:** Kubernetes with Multus CNI and Whereabouts IPAM; small IP pool intentionally exhausted during Pod replacement.
- **Symptoms:** New Pod remains ContainerCreating after CNI ADD fails due to IP exhaustion, even after the old Pod releases its address.
- **Root-cause evidence:** CNI maintainer discussion points to a failing DelegateDel path: CNI DEL should be permissive and clean up what it can; an IPAM plugin error during deletion can prevent sandbox cleanup. The reporter said Whereabouts IPAM would be fixed and the issue was closed.
- **Resolution:** Fix IPAM DEL behavior and validate address release/cleanup. This is not evidence that kubelet should blindly retry every failed CNI ADD.
- **Lab safety:** Safe only with a disposable cluster and isolated IP pool; never use production address ranges.
- **Learning path:** Pod sandbox lifecycle → CNI ADD/DEL contract → Multus delegation → IPAM allocation and cleanup.
