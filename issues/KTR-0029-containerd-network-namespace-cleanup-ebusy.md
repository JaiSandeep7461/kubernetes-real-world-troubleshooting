# KTR-0029 — Pod deletion fails when CNI network namespace cannot be removed

- **Source:** [containerd/containerd#3667](https://github.com/containerd/containerd/issues/3667)
- **Area:** containerd CRI / CNI DEL / Linux network namespaces and mounts
- **Evidence:** Issue and maintainer comments reviewed; multiple environment-specific workarounds were reported.
- **Environment:** containerd 1.2.9; CentOS 7 and CSI driver mount-propagation configurations were discussed.
- **Symptoms:** Pod remains Terminating. PodSandboxStatus fails while removing a network namespace with `device or resource busy`.
- **Root-cause evidence:** Maintainer noted that `ip netns` mounts over `/run/netns` and can cover an existing network namespace; other users reported CSI mount-propagation interactions and a workaround using `fs.may_detach_mounts`. The exact cause depends on host mount and namespace state.
- **Resolution:** Validate host mount propagation and iproute2 behavior; one reported workaround was enabling `fs.may_detach_mounts`. Do not apply sysctl changes blindly.
- **Lab safety:** Use a disposable VM; namespace and mount cleanup changes can affect host networking.
- **Learning path:** network namespaces → bind mounts → mount propagation → CNI DEL → CRI PodSandboxStatus.
