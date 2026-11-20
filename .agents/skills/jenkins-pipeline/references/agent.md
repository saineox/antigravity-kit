# Agent Diagnosis — Jenkins Pipeline

## Kubernetes Agent Provisioning

1. Jenkins uses K8s plugin to create ephemeral Pod agents.
2. Agent template defines container images, volumes, resources.
3. Pod is created → JNLP connects to controller → Pipeline runs → Pod deleted.

## Agent Not Starting

| Symptom | Check |
|---------|-------|
| Pod never created | K8s plugin config, cloud config, RBAC |
| Pod pending | Node scheduling, resource quota, PVC binding |
| Pod running but not connecting | JNLP port, network policy, DNS |
| Pod CrashLoopBackOff | Agent image, JNLP version, memory |
| Timeout waiting for agent | All of the above + controller URL reachable |

## Agent Template Issues

1. Image must contain JNLP agent or use `inbound-agent`.
2. Working directory must be writable.
3. ServiceAccount must have permissions in the namespace.
4. Resource requests/limits must be set (avoid unbounded agents).
5. Volume mounts for workspace persistence if needed.

## OpenShift-Specific

- Agent Pods run with assigned SCC (usually `restricted`).
- Random UID assignment — files must be world-readable.
- Check if `anyuid` SCC is required (WARN if recommending).

## Evidence Commands

```bash
oc get pods -n <jenkins-ns> -l jenkins/label=<agent-label>
oc describe pod <agent-pod> -n <jenkins-ns>
oc get events -n <jenkins-ns> --sort-by=.lastTimestamp
```
