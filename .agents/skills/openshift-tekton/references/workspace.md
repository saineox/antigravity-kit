# Workspace & Storage — Tekton

## Workspace Types

| Type | Use Case | Persistence |
|------|----------|------------|
| `volumeClaimTemplate` | Per-PipelineRun PVC, auto-created | Deleted after run |
| `persistentVolumeClaim` | Shared PVC across runs | Persistent |
| `emptyDir` | Temporary, fast, no persistence | Pod lifetime only |
| `configMap` | Read-only config files | Persistent (read-only) |
| `secret` | Credentials (read-only) | Persistent (read-only) |

## PVC Issues

1. PVC pending → check StorageClass and provisioner.
2. Access mode mismatch → ReadWriteOnce limits to single node.
3. Multiple TaskRuns on different nodes need ReadWriteMany.
4. Capacity exceeded → check PVC size.

## Permission Denied

1. OpenShift assigns random UIDs — files owned by root may be unreadable.
2. Check `fsGroup` in SecurityContext.
3. Check SCC assignment on the ServiceAccount.
4. `initContainers` can fix permissions before Steps run.

## Evidence Commands

```bash
oc get pvc -n <ns>
oc describe pvc <name> -n <ns>
oc get sc
oc get pod <pod> -n <ns> -o yaml | grep -A10 securityContext
```
