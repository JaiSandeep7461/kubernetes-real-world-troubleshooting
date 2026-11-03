# KTR-0028 — Pod remains Terminating because containerd retains a zombie container

- **Source:** [containerd/containerd#11133](https://github.com/containerd/containerd/issues/11133)
- **Area:** containerd shim / task exit processing / Pod termination
- **Evidence:** Issue discussion and merged PR #10651 reviewed. Maintainer explicitly separated the original containerd exit-processing bug from later io_uring and RCU kernel bugs.
- **Environment:** GKE clusters; containerd 1.7.x and Linux kernel variants were discussed.
- **Symptoms:** A Pod remains Terminating because containerd still believes a container is running although the process has exited; exec probes may fail with “cannot exec in a stopped container”.
- **Root cause:** A race in shim exit processing could drop init exit notifications, particularly with execs in flight and containers without a private PID namespace.
- **Resolution:** containerd PR #10651 merged 2024-09-05 and rewrote shim exit processing to track running execs and avoid dropping init exits. Separate kernel issues require separate diagnosis.
- **Lab safety:** Historical bug; use a version-pinned disposable VM if studying regression behavior.
- **Learning path:** PID namespaces → containerd shim → exec process tracking → task exit notifications → Pod termination.
