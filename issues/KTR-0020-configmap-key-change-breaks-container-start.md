# KTR-0020 — ConfigMap key change leaves a mounted file deleted and container restart fails

- **Upstream:** [kubernetes/kubernetes#86776](https://github.com/kubernetes/kubernetes/issues/86776)
- **Module:** 01 — Pod foundations (container startup/configuration dependency)
- **Concepts:** ConfigMap volumes, projected files, container restart, mount behavior, application file dependencies
- **Evidence:** Reporter identified an erroneous ConfigMap change as the cause and confirmed deleting the affected Pods restored operation.
- **Status:** Configuration-failure learning case; not a confirmed Kubernetes defect.

## Symptom
After a container was OOMKilled, its restart failed with an OCI runtime message: `container init exited prematurely`. Sidecars continued running. The reporter also observed a ConfigMap-mounted file reported as deleted from inside the container.

## Root cause found by reporter
An erroneous ConfigMap change removed or altered a key that the container expected through a mounted file. The reporter identified the missing/deleted mounted file as the actual cause. The issue was closed at the reporter's request.

## Reproduction outline
1. Create a ConfigMap containing a file key required by a test application.
2. Mount the key into a Pod at the expected file path.
3. Make the application read that file during startup.
4. Change the ConfigMap so the required key is removed or renamed.
5. Observe the mounted-file behavior and application/container startup logs.
6. Correct the ConfigMap key and recreate the Pod.

Use a disposable test namespace and a simple test image. Validate exact mount semantics on the chosen Kubernetes version; avoid assuming that all ConfigMap mount styles update identically.

## Verification
The required file exists at the expected path, the application starts, and the container reaches Running/Ready after the corrected Pod is created.

## Learning value
This is a configuration mistake rather than a Kubernetes product bug. It is useful because real troubleshooting must distinguish runtime symptoms from the configuration change that caused them. The original environment was Kubernetes 1.14.6 with Docker, so current-version behavior must be checked.
