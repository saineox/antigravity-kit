# Credentials — Jenkins Pipeline

## Credential Types

| Type | Use Case |
|------|----------|
| Username/Password | Git, registry, API |
| SSH Private Key | Git SSH, server access |
| Secret Text | API tokens, webhook secrets |
| Certificate | Mutual TLS |
| Kubernetes ServiceAccount | K8s API access |

## Credential Binding

```groovy
withCredentials([usernamePassword(
    credentialsId: 'my-cred',
    usernameVariable: 'USER',
    passwordVariable: 'PASS')]) {
    // Use $USER and $PASS — Jenkins masks them in logs
}
```

## Common Failures

| Symptom | Cause |
|---------|-------|
| "No such credential" | Wrong ID, wrong scope, credential deleted |
| "Access denied" | Credential exists but Jenkins RBAC blocks access |
| Clone fails with 401 | Wrong credential type or expired token |
| Push fails with 403 | Token lacks write permission |

## Security Rules

- NEVER interpolate credentials in Groovy string interpolation (`"$password"`).
- Use `withCredentials` binding instead.
- Disable shell tracing (`set +x`) when credentials are used in shell steps.
- Check credential scope: System vs Global vs Folder.
