# Research methodology

## Sources
Prioritize original GitHub issues and pull requests in Kubernetes and ecosystem projects, Kubernetes SIG discussions, official project documentation/release notes, and well-specified incident reports. Stack Overflow and forums are useful discovery sources, not automatic proof of a product defect.

## Candidate-to-resolution lifecycle
1. **Candidate** — discovered by search; not yet checked against the original source.
2. **Source validated** — original report fetched; title, body, status, symptoms, and reported environment inspected.
3. **Root cause partial** — evidence narrows the cause but leaves meaningful uncertainty.
4. **Root cause confirmed** — maintainer analysis, linked PR/commit, regression test, or equivalent evidence supports the explanation.
5. **Lab-ready** — bounded reproduction plan, versions, prerequisites, risks, and expected signals reviewed.
6. **Reproduced** — the failure was actually observed in the named environment.
7. **Solved & verified** — remediation was applied and expected behavior independently verified.

## Required record fields
Record ID; source repository and issue number; title and URL; primary area; issue type; issue status; evidence status; symptoms; versions/environment; concept tags; Linux dependencies; root-cause evidence; resolution evidence; reproduction fit; difficulty; freshness; next research action.

## Validation checklist
- Fetch the original issue, not just search snippets.
- Read comments, maintainer responses, linked PRs/commits, and relevant release notes.
- Distinguish symptom, hypothesis, confirmed root cause, workaround, and permanent fix.
- Record exact versions and topology. Mark missing details as unknown.
- Check whether the report is a bug, configuration error, operational question, documentation gap, or feature request.
- Deduplicate by failure mode while retaining meaningful differences in versions and environments.
- Assess whether the behavior still applies to supported/current releases.
- Never infer a fix from issue closure alone.

## Reproduction safety
No labs are part of the initial collection phase. Before a case becomes lab-ready, define target environment (KIND, disposable Linux VM, or cloud), version constraints, prerequisites, destructive risk, observable failure signal, evidence commands, rollback, and post-fix validation. Do not run experiments on production infrastructure.

## Scope and limitations
There is no finite public index of every Kubernetes-related incident. Private incidents are inaccessible and public repositories continuously change. The project therefore aims for broad, transparent, source-linked coverage—not a claim of exhaustive collection. Candidate counts must never be presented as validated cases.