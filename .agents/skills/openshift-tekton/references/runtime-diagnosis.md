# Runtime Diagnosis — Tekton

## PipelineRun Failed

1. Check PipelineRun status: `oc get pipelinerun <name> -n <ns>`.
2. Identify which TaskRun failed: `oc get pipelinerun <name> -n <ns> -o yaml`.
3. Check the failed TaskRun's Pod.
4. Check Pod events, container logs, exit codes.

## TaskRun Debugging

1. Get TaskRun Pod: `oc get pods -n <ns> -l tekton.dev/taskRun=<taskrun>`.
2. Each step = one container. Check the right container.
3. Logs: `oc logs <pod> -c step-<name> -n <ns>`.
4. Previous: `oc logs <pod> -c step-<name> -n <ns> --previous`.
5. Events: `oc get events -n <ns> --sort-by=.lastTimestamp`.

## Common Failures

| Symptom | Cause |
|---------|-------|
| TaskRun Pod pending | PVC not bound, node scheduling, resource quota |
| Step exit code 1 | Script/command failure — read step logs |
| Step exit code 126 | Script not executable — check permissions |
| Step exit code 127 | Command not found — wrong image or missing tool |
| Step exit code 137 | OOMKilled — increase memory limit |
| Timeout | Pipeline/Task timeout exceeded |
| ImagePullBackOff | Wrong image, missing pull secret, registry error |

## Read-Only Evidence Commands

```bash
oc get pipelinerun -n <ns>
oc describe pipelinerun <name> -n <ns>
tkn pipelinerun describe <name> -n <ns>
tkn pipelinerun logs <name> -n <ns>
oc get events -n <ns> --sort-by=.lastTimestamp
```
