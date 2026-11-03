# KTR-0018 — Missing imagePullSecret warning emitted even when image pull succeeds

- **Upstream:** [kubernetes/kubernetes#128544](https://github.com/kubernetes/kubernetes/issues/128544)
- **Fix:** [kubernetes/kubernetes#138432](https://github.com/kubernetes/kubernetes/pull/138432) — merged 2026-06-26
- **Module:** 01 — Pod foundations (image pull and event diagnosis)
- **Concepts:** imagePullSecrets, ServiceAccount image pull configuration, kubelet events, image pull success/failure
- **Evidence:** Maintainer discussion and merged fix confirm the behavior and resolution.
- **Status:** Lab-ready after choosing a release containing the merged fix.

## Symptom
A Pod referenced a nonexistent image pull Secret, but the image was still pulled successfully (for example, because the image was public or another credential source worked). Kubelet nevertheless emitted repeated `FailedToRetrieveImagePullSecret` warnings. The report described thousands of warning events despite a healthy running Pod.

## Root cause
The warning was emitted for a missing referenced Secret even when the missing Secret did not cause an image pull failure.

## Resolution
PR #138432 changed the behavior so the warning is emitted only when image pulling has failed and the missing Secret may be relevant.

## Reproduction outline
1. Create a test namespace.
2. Create a Pod that references a deliberately nonexistent imagePullSecret.
3. Use a public image that can be pulled without credentials.
4. Inspect Pod events and confirm the Pod reaches Running despite the missing-secret warning on an affected release.
5. Repeat on a release containing PR #138432 and compare event behavior.

## Verification
The public image pulls successfully. On a fixed release, no misleading missing-secret warning is emitted when image pulling succeeds. Then test with a genuinely private image and missing credentials to confirm a relevant pull failure is still reported.

## Learning value
This is not an application outage. It teaches the difference between a warning symptom and a failed workload, and why event streams must be interpreted alongside Pod status and image pull results.
