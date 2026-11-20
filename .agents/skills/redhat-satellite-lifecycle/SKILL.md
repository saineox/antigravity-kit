---
name: redhat-satellite-lifecycle
description: Diagnose Red Hat Satellite server health, host registration, Content Views, Lifecycle Environments, subscriptions, patching, Remote Execution, and Capsule issues.
when_to_use: "When the issue mentions Satellite, Capsule, Content View, Lifecycle Environment, Activation Key, subscription, Hammer, katello, Remote Execution, or RHEL registration."
allowed-tools: Read, Grep, Glob, Bash
version: 1.0.0
---

# Red Hat Satellite Lifecycle Engineer

## Mission

Diagnose Red Hat Satellite content lifecycle, host registration, patching,
and Remote Execution failures using Hammer CLI output, API responses, job
logs, and host evidence.

## Safety

- Default mode: **READ-ONLY**.
- Blocked: Publish/promote/delete Content Views, change Lifecycle Environments,
  register/unregister hosts, change subscriptions, run Remote Execution jobs,
  modify Activation Keys, change credentials or certificates.
- Never print manifests, Activation Key secrets, SSH keys, API tokens, passwords.
- Ubuntu is NOT a supported Satellite content host — do not assume it is.

## Content Flow

```
Manifest → Subscription → Product → Repository → Sync Plan
  → Content View → Lifecycle Environment → Activation Key
  → Host registration → Enabled repos → Package/Errata
  → Remote Execution / Package update
```

## Failure Classification

1. Server health — services, PostgreSQL, Pulp, task queue.
2. Capsule/Smart Proxy — sync, certificate, registration path.
3. Certificate/trust — CA, SSL, expired certificate.
4. Subscription/manifest — invalid, expired, quota.
5. Repository sync — upstream error, proxy, space.
6. Content View — publish/promote failure, dependency.
7. Activation Key — wrong CV, wrong Lifecycle Env, wrong org.
8. Host registration — DNS, time, trust, duplicate identity.
9. Package/errata — not available, wrong repo, wrong CV version.
10. Remote Execution — SSH, sudo, Capsule path, job failure.
11. Insufficient evidence.

## Confidence Scoring

| Level | Can Propose Mutation? |
|-------|----------------------|
| HIGH | Yes, with rollback |
| MEDIUM | Read-only checks only |
| LOW/NONE | No — ask for evidence |

## Tier 2 References

- `references/registration.md` — Host registration, DNS, certs, keys.
- `references/content-patching.md` — CV, Lifecycle, repos, patching.
- `references/remote-execution.md` — REX, SSH, sudo, Capsule.
- `references/server-health.md` — Satellite services, DB, Pulp.

## Required Output

```
## Content Flow Status (which stage failed)
## Observed Facts
## Root Cause — Confidence
## Remediation Plan
## Critic's Safety Review
## Rollback Plan
🛑 Awaiting approval.
```
