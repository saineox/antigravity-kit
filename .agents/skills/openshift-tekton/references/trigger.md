# Trigger Diagnosis — Tekton

## Trigger Flow

```
External event (webhook) → EventListener (Service/Route)
  → TriggerBinding (extract params) → TriggerTemplate (create PipelineRun)
```

## EventListener Not Receiving Events

1. Check EventListener pod: `oc get pods -n <ns> -l eventlistener=<name>`.
2. Check EventListener Service and Route.
3. Check Route TLS and DNS.
4. Check firewall/proxy between Git provider and EventListener.
5. Check webhook secret matches.

## TriggerBinding Failures

1. Check params match the incoming event payload structure.
2. JSON path expressions must match the actual webhook payload.
3. GitHub, GitLab, and Bitbucket have different payload structures.
4. Use CEL interceptors for filtering (only trigger on specific branches).

## PipelineRun Not Created

1. TriggerTemplate may have invalid PipelineRun spec.
2. ServiceAccount in TriggerTemplate may lack RBAC to create PipelineRuns.
3. Check EventListener pod logs for error details.

## Evidence Commands

```bash
oc get eventlistener -n <ns>
oc describe eventlistener <name> -n <ns>
oc get pods -n <ns> -l eventlistener=<name>
oc logs -n <ns> -l eventlistener=<name>
oc get route -n <ns>
```
