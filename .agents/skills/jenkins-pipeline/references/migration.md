# Jenkins ↔ Tekton Migration — Jenkins Pipeline

## When to Use This Reference

Load only when the user asks about migrating from Jenkins to Tekton,
comparing them, or correlating issues across both systems.

## Mapping Table

| Jenkins Concept | Tekton Equivalent |
|----------------|-------------------|
| Pipeline | Pipeline |
| Stage | Task |
| Step | Step |
| Agent | PipelineRun ServiceAccount + Pod |
| Shared Library | ClusterTask or Tekton Hub Task |
| Credential | Kubernetes Secret + ServiceAccount |
| Jenkinsfile | Pipeline YAML |
| Plugin | Custom Task image |
| Post actions | `finally` Tasks |

## What Does NOT Map Cleanly

1. **Shared Libraries** — require redesign as container images or Tasks.
2. **CPS/Groovy logic** — Tekton uses YAML + shell, not Groovy.
3. **Controller-side logic** — Tekton is fully distributed, no controller.
4. **Human approvals** — Tekton uses `CustomRun` or external approval.
5. **Plugin ecosystem** — must be replaced with container-based tooling.

## Migration Analysis Checklist

1. List all Jenkins stages → map to Tekton Tasks.
2. Identify Shared Libraries → plan redesign.
3. Map credentials → Kubernetes Secrets.
4. Identify workspace behavior differences.
5. Identify retry semantics and failure notification.
6. Verify rollback behavior.

## Warning

Do NOT claim that converting a Jenkinsfile line-by-line produces an
equivalent Tekton pipeline. The architectures are fundamentally different.
