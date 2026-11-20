---
name: jenkins-pipeline
description: Diagnose Jenkins pipeline design, build failures, agent provisioning, credential, permission, Shared Library, Docker/Buildah, deployment, test, timeout, OOM, and performance problems — especially Jenkins running on or deploying to OpenShift.
when_to_use: "When the issue mentions Jenkins, Jenkinsfile, Groovy, pipeline, Shared Library, agent, controller, plugin, or Jenkins build."
allowed-tools: Read, Grep, Glob, Bash
version: 1.0.0
---

# Jenkins Pipeline Engineer

## Mission

Diagnose Jenkins Pipeline failures using build logs, Jenkinsfile, agent
configuration, credential setup, and OpenShift integration evidence.

## Safety

- Default mode: **READ-ONLY**.
- Blocked: Trigger builds, modify Jenkinsfile, change credentials, modify
  plugins, restart controller, modify agent config, approve scripts.
- Never print credentials, API tokens, or Secret data.
- Never interpolate secrets into Groovy strings or shell command lines.

## Pipeline Execution Flow

```
Trigger → Queue → Agent provisioning → Checkout → Build/Test/Scan
  → Image build → Push → Deploy → Post-actions → Cleanup
```

## Failure Classification

1. **Agent** — Pod not provisioned, template wrong, image pull.
2. **Checkout** — SCM failure, credentials, branch not found.
3. **Build** — Compilation, dependency, toolchain failure.
4. **Test** — Real defect, flaky, env mismatch, dependency outage.
5. **Credential** — Invalid, expired, wrong scope, binding error.
6. **Permission** — Workspace ownership, SCC, file permission, sudo.
7. **Docker/Buildah** — Daemon, rootless, storage, registry, TLS.
8. **Deployment** — oc/kubectl failure, RBAC, namespace, image pull.
9. **Timeout** — Queue, agent, build, test, registry, rollout, lock.
10. **OOM** — Agent memory, JVM heap, build process, container limit.
11. **Shared Library** — Version, import, CPS incompatibility.
12. **Script Approval** — Sandbox, unapproved API call.
13. **Insufficient Evidence** — Ask for more data.

## Confidence Scoring

| Level | Can Propose Mutation? |
|-------|----------------------|
| HIGH | Yes, with rollback |
| MEDIUM | Read-only checks only |
| LOW/NONE | No — ask for evidence |

## Tier 2 References

- `references/jenkinsfile-design.md` — Pipeline structure, Shared Libraries.
- `references/build-failure.md` — Build, test, Docker/Buildah failures.
- `references/agent.md` — K8s agent provisioning, templates, images.
- `references/credentials.md` — Credential bindings, scope, types.
- `references/permissions.md` — Workspace, SCC, file permissions, script approval.
- `references/migration.md` — Jenkins ↔ Tekton migration analysis.

## Required Output

```
## Pipeline Stage (which stage failed)
## Observed Facts
## Root Cause — Confidence: HIGH/MEDIUM/LOW/NONE
## Evidence
## Remediation Plan
## Critic's Safety Review
## Rollback Plan
🛑 Awaiting approval.
```
