# Jenkinsfile Design — Jenkins Pipeline

## Declarative vs Scripted

- **Declarative**: Preferred. Structured, easier to maintain, better error messages.
- **Scripted**: Only when Declarative cannot express the logic.
- Never mix styles in the same Jenkinsfile.

## Shared Libraries

- Use `@Library('name@version')` — always pin versions.
- Global vs folder-level libraries.
- CPS (Continuation Passing Style) restrictions: no closures in pipeline body.
- Shared Library changes can break all consuming pipelines — test first.

## Stage Structure

```groovy
pipeline {
    agent { kubernetes { /* template */ } }
    options { timeout(time: 30, unit: 'MINUTES') }
    stages {
        stage('Build') { steps { /* */ } }
        stage('Test')  { steps { /* */ } }
        stage('Deploy') { steps { /* */ } }
    }
    post {
        failure { /* notification */ }
        always  { /* cleanup */ }
    }
}
```

## Best Practices

- One stage per logical operation.
- Timeout at pipeline AND stage level.
- Use `post` blocks for cleanup and notification.
- Avoid `input` steps inside agent blocks (holds agent while waiting).
- Use `stash`/`unstash` to pass artifacts between stages on different agents.
