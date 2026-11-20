# Build & Test Failure — Jenkins Pipeline

## Build Failure

1. Check console output — find the FIRST error, not the last.
2. Distinguish: compilation error vs dependency error vs toolchain error.
3. Check build tool version (Maven, Gradle, npm, Go).
4. Check dependency resolution (proxy, mirror, registry).
5. Check environment variables available to the build.

## Test Failure Classification

| Type | Fix |
|------|-----|
| Real defect | Fix the code |
| Flaky test | Isolate and fix, never auto-retry to hide |
| Env mismatch | Fix agent image or config |
| Dependency outage | Check external service availability |
| Test data | Reset or regenerate test fixtures |
| Timing/race | Add proper waits, not sleep() |
| Resource limit | Check container limits |
| Parallel interference | Isolate test namespaces |

## Docker/Buildah Failure

1. Daemon availability (Docker) or rootless mode (Buildah).
2. Storage driver and disk space.
3. Registry authentication (pull/push).
4. TLS certificates and proxy.
5. Image architecture (amd64 vs arm64).
6. OpenShift SCC restrictions — prefer rootless Buildah over privileged Docker.

## Evidence

- Full console output (not truncated).
- Jenkinsfile.
- Build tool configuration (pom.xml, package.json, Dockerfile).
