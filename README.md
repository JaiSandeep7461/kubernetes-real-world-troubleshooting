# Kubernetes Real-World Troubleshooting Database

A source-linked, evidence-graded collection of real Kubernetes and ecosystem failure reports. The goal is to learn troubleshooting through actual incidents, cross-layer dependency analysis, and eventually controlled reproductions.

## Scope
Linux and kernel; networking; OCI/container runtimes; Kubernetes architecture and core; CNI and dataplanes; DNS; ingress and Gateway API; storage and CSI; scheduling and autoscaling; security and identity; observability; and managed cloud platforms (AKS, EKS, GKE).

## Important scope boundary
There is no finite, authoritative list of every Kubernetes issue worldwide. This repository is a continuously expandable research collection, not a claim of exhaustive coverage. Search results are candidates until the original source has been inspected.

## Evidence rules
- A closed issue does not prove that its root cause was fixed.
- A user question or support request is not automatically a product defect.
- Root cause confidence is tracked separately from issue status.
- Historical reports remain useful for fundamentals but are labelled by version and relevance.
- Similar reports are deduplicated by failure mode, while preserving distinct environments and evidence.
- Reproduction and verification are never claimed until actually performed.

## Repository layout
- `data/issue-register.csv` — searchable master register.
- `docs/concept-taxonomy.md` — major concepts and cross-layer relationships.
- `docs/dependency-map.csv` — common failure paths across components.
- `docs/research-methodology.md` — collection, validation, and evidence rules.
- `issues/` — individual case files as the collection grows.

## Evidence status vocabulary
- `Candidate`: discovered in search; original source not yet checked.
- `Source validated`: original issue exists; report and environment inspected.
- `Root cause partial`: plausible explanation supported by some evidence, but incomplete.
- `Root cause confirmed`: maintainer, linked fix, test, or equivalent evidence supports the cause.
- `Lab-ready`: safe, bounded reproduction plan reviewed; not yet reproduced.
- `Reproduced`: failure observed in the specified environment.
- `Solved & verified`: remediation applied and expected behavior independently checked.

## Current baseline
The initial batch contains 13 source-validated GitHub issues. Root-cause confidence varies; do not interpret all 13 as confirmed defects or solved cases. Labs are intentionally not started yet.

## Working sequence
1. Expand taxonomy and source coverage.
2. Collect candidate reports from primary sources.
3. Inspect original issue, comments, linked PRs, releases, and relevant documentation.
4. Classify issue type, version relevance, evidence confidence, and duplicates.
5. Map dependencies and reproduction constraints.
6. Review collection coverage before selecting any lab.

See the CSV register and documentation for record-level details.

## Learning roadmap

The ordered 150-case plan is documented in [`docs/150-case-learning-roadmap.md`](docs/150-case-learning-roadmap.md). The initial source-linked case catalogue is in [`issues/README.md`](issues/README.md), with detailed cases under `issues/` and the searchable register in `data/issue-register.csv`.
