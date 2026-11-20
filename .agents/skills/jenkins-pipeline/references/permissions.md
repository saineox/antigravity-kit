# Permissions — Jenkins Pipeline

## Workspace Ownership

1. Jenkins agent runs as specific UID inside the container.
2. OpenShift assigns random UIDs — files owned by root may fail.
3. Check if workspace directory is writable by the container UID.
4. Check volume mount mode (readOnly vs readWrite).

## Script Approval

1. Jenkins sandbox blocks unsafe Groovy API calls.
2. Unapproved calls fail with `Scripts not permitted to use...`.
3. Do NOT blindly approve — explain the API call and security impact.
4. Check if the call can be replaced with a safe alternative.

## SCC on OpenShift

1. Agent ServiceAccount gets default SCC (usually `restricted-v2`).
2. If agent needs root or specific capabilities → SCC escalation needed.
3. WARN before recommending `anyuid` or `privileged`.
4. Check if rootless alternatives exist (Buildah vs Docker).

## File Permissions in Steps

- `chmod +x script.sh` may be needed.
- SELinux labels on RHEL may block access.
- Check `fsGroup` in SecurityContext.
- Git clone preserves file modes — check source repo permissions.
