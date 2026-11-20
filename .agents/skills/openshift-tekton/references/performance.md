# Performance — Tekton

## Bottleneck Identification

Measure each phase separately:

1. **Trigger → PipelineRun creation** — should be seconds.
2. **Pod scheduling** — check node pressure, PVC binding.
3. **Image pull** — check image size, registry latency, caching.
4. **Step execution** — the actual build/test time.
5. **Between Tasks** — overhead from Pod creation per Task.

## Optimization Strategies

| Bottleneck | Fix |
|-----------|-----|
| Slow image pull | Use image caching, smaller images, pre-pull DaemonSet |
| Sequential Tasks that could parallel | Use `runAfter` only for true dependencies |
| Large git clone | Use shallow clone, sparse checkout |
| Dependency download every run | Cache in persistent workspace |
| Too many Task Pods | Combine related Steps into fewer Tasks |

## Timeouts

- Set `pipeline.spec.timeouts.pipeline` for overall timeout.
- Set `pipeline.spec.timeouts.tasks` for per-task timeout.
- Set `pipeline.spec.timeouts.finally` for cleanup timeout.
- Without timeouts, stuck runs consume resources indefinitely.

## Anti-Patterns

- Never disable timeouts to "fix" slow pipelines — find the bottleneck.
- Never add automatic retries for deterministic failures.
- Never share workspaces across concurrent PipelineRuns without ReadWriteMany.
