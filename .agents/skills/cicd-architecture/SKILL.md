---
name: cicd-architecture
description: Correlate cross-system CI/CD failures spanning Jenkins, Tekton, ArgoCD, container registries, and OpenShift deployments.
when_to_use: "When a failure crosses two or more systems — e.g., build succeeds but deploy fails, image pushed but not pulled, sync succeeds but pods crash."
allowed-tools: Read, Grep, Glob, Bash
version: 1.0.0
---

# CI/CD Architecture Correlator

## Mission

Correlate failures that span multiple CI/CD systems. Loaded ONLY when the
openshift-sre agent detects keywords from 2+ domains in a single incident.

## End-to-End Pipelines

### Jenkins → OpenShift

```
Git commit → Jenkins webhook → Pipeline → Build/Test/Scan
  → Image push → Registry → GitOps repo update
  → ArgoCD sync → OpenShift rollout
```

### Tekton → OpenShift

```
Git commit → Tekton Trigger → PipelineRun → TaskRuns
  → Image push → Registry → GitOps repo update
  → ArgoCD sync → OpenShift rollout
```

## Cross-System Failure Patterns

| Symptom | Systems Involved | Investigation |
|---------|-----------------|---------------|
| Build succeeds, deploy fails | CI + OpenShift | Check image tag, registry auth, namespace RBAC |
| Image pushed, OpenShift can't pull | CI + Registry + OpenShift | Check registry URL, pull secret, image name/tag |
| ArgoCD sync OK, pods crash | ArgoCD + OpenShift | Sync delivered bad manifest — check app config |
| Webhook fires, no pipeline starts | Git + CI | Check webhook URL, secret, EventListener/Jenkins config |
| Pipeline passes, wrong env deployed | CI + GitOps | Check branch-to-environment mapping |

## Correlation Rules

1. Identify the FIRST system that failed.
2. Trace the data flow forward from that point.
3. Each system boundary is a potential failure point.
4. Check: credentials, DNS, TLS, image tags, namespace, RBAC at each boundary.
5. Never blame downstream systems for upstream failures.

## Safety

- This skill is READ-ONLY analysis only.
- It does not propose mutations — it identifies which domain skill should handle the fix.
- It does not load Tier 2 references — those come from the domain skills.
