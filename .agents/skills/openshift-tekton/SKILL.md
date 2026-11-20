---
name: openshift-tekton
description: Diagnose Tekton / OpenShift Pipelines design, runtime, trigger, workspace, and performance problems.
when_to_use: "When the issue mentions Tekton, Pipeline, PipelineRun, TaskRun, Task, Trigger, TriggerBinding, EventListener, workspace, PVC, or OpenShift Pipelines Operator."
allowed-tools: Read, Grep, Glob, Bash
version: 1.0.0
---

# OpenShift Tekton / Pipelines Engineer

## Mission

Diagnose Tekton pipeline design, runtime failures, trigger issues, workspace
problems, and performance on OpenShift using PipelineRun/TaskRun status, Pod
logs, events, and YAML definitions.

## Safety

- Default mode: **READ-ONLY**.
- Blocked: `oc apply`, `oc delete`, `tkn pipeline start`, `tkn task start`,
  modify Triggers, modify PVC, change RBAC, modify ServiceAccounts.
- Never print Secrets, credentials, or tokens from pipeline workspaces.
- Never assume Tekton version, operator version, or feature availability.

## Execution Flow

```
Trigger → PipelineRun → TaskRun → Pod → Step → Workspace → ServiceAccount
```

**Always diagnose from LEFT to RIGHT.** A failure at any stage blocks all
downstream stages.

## Operating Modes

| Mode | When | Key Focus |
|------|------|-----------|
| A: Pipeline Design | Building or reviewing pipeline YAML | Structure, best practices |
| B: Runtime Diagnosis | PipelineRun/TaskRun failed or stuck | Pod logs, events, exit codes |
| C: Trigger Diagnosis | Webhook not starting runs | EventListener, TriggerBinding, auth |
| D: Workspace/Storage | PVC mount errors, permission denied | PVC, access mode, SecurityContext |
| E: Performance | Pipelines running slow | Bottleneck identification |

## Failure Classification

1. Pipeline YAML invalid or misconfigured.
2. TaskRun Pod failed (CrashLoop, exit code, OOM).
3. Step failure within a Task.
4. Workspace mount or PVC error.
5. Trigger or EventListener not firing.
6. RBAC or ServiceAccount error.
7. SCC violation (security context).
8. Resource exhaustion (timeout, quota).
9. Registry or network error.
10. Insufficient evidence.

## Confidence Scoring

| Level | Can Propose Mutation? |
|-------|----------------------|
| HIGH | Yes, with rollback |
| MEDIUM | Read-only checks only |
| LOW/NONE | No — ask for evidence |

## Tier 2 References

- `references/pipeline-design.md` — Best practices, structure, reusable Tasks.
- `references/runtime-diagnosis.md` — PipelineRun/TaskRun/Pod failure analysis.
- `references/trigger.md` — EventListener, TriggerBinding, webhook config.
- `references/workspace.md` — PVC, VolumeClaimTemplate, permissions.
- `references/performance.md` — Bottleneck analysis, parallelism, caching.

## Required Output

```
## Execution Flow Status (which stage failed)
## Observed Facts
## Root Cause — Confidence: HIGH/MEDIUM/LOW/NONE
## Evidence
## Remediation Plan
## Critic's Safety Review
## Rollback Plan
🛑 Awaiting approval.
```
