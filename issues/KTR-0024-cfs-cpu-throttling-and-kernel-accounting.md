# KTR-0024 — CPU limits cause unexpected CFS throttling

- **Sources:** [kubernetes/kubernetes#67577](https://github.com/kubernetes/kubernetes/issues/67577) and [follow-up #97445](https://github.com/kubernetes/kubernetes/issues/97445)
- **Area:** Pod CPU limits / cgroups / Linux CFS bandwidth control
- **Evidence:** Original discussion and follow-up reviewed. Original issue was closed after Linux kernel fixes were identified; follow-up reports show later throttling can have different workload/accounting causes.
- **Environment:** Original reports included Kubernetes CPU limits and older Linux kernels; follow-up includes AWS/EKS kernels and Ubuntu 20.04/Linux 5.4.
- **Symptoms:** Containers experience throttling despite apparently low average CPU usage or single-threaded workloads; increasing CPU limits can reduce observed throttling.
- **Root-cause evidence:** Original discussion points to Linux CFS bandwidth accounting bugs and kernel fixes. Follow-up discussion also identifies short, microscopic CPU bursts that may be invisible to coarse Prometheus/cAdvisor sampling. Do not assume all throttling is the same kernel bug.
- **Resolution:** Use an appropriate supported kernel; inspect cgroup throttling counters and workload behavior. Tune requests/limits based on measured workload rather than removing limits blindly.
- **Lab safety:** Run bounded CPU tests in a disposable cluster; never remove resource limits from shared or production nodes as an experiment.
- **Learning path:** requests vs limits → cgroups CPU shares/quota → CFS periods → throttling counters → kernel scheduler behavior.
