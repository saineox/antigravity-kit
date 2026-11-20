# Pipeline Design — Tekton

## Structure Best Practices

1. Each Task does ONE thing (build, test, scan, deploy).
2. Pipelines compose Tasks with `runAfter` for ordering.
3. Use `params` for all configurable values — never hardcode.
4. Use `results` to pass data between Tasks.
5. Use `finally` Tasks for cleanup (runs regardless of failure).

## Task Reuse

- Prefer Tekton Hub Tasks for standard operations (git-clone, buildah).
- Pin Task versions with `taskRef.resolver` or bundle references.
- Custom Tasks only when Hub Tasks don't cover the use case.

## Parameter Flow

```
Pipeline params → Task params → Step env/args
```

Always validate: parameter type (string vs array), required vs optional,
and default values.

## Workspace Naming

- Use descriptive names: `source-code`, `build-cache`, `docker-config`.
- Bind workspaces at PipelineRun level, not hardcoded in Pipeline.

## Anti-Patterns

| Anti-Pattern | Correct Approach |
|-------------|-----------------|
| All logic in one Task | Split into focused Tasks |
| Hardcoded image tags | Use params for image:tag |
| No finally Tasks | Add cleanup and notification |
| Inline scripts > 10 lines | Move to ConfigMap or image |
| No timeouts | Set pipeline and task timeouts |
