# Remote Execution — Red Hat Satellite

## REX Prerequisites

1. Host registered with Satellite.
2. Host reachable via SSH from Capsule/Smart Proxy.
3. SSH credentials configured (key-based preferred).
4. Remote execution feature enabled on the Capsule.
5. Correct Smart Proxy path configured for the host.

## Common Failures

| Symptom | Cause |
|---------|-------|
| "Host unreachable" | DNS, firewall, host down, SSH port blocked |
| "SSH authentication failed" | Wrong key, key permissions, authorized_keys missing |
| "Sudo: no tty present" | Sudo requires TTY, configure `!requiretty` |
| "Permission denied" | SELinux denying, wrong effective user |
| "Job template not found" | Template name wrong or not synced to Capsule |
| "Capsule error" | Smart Proxy down, proxy not assigned to host |

## Job Investigation

1. Check job status: `hammer job-invocation list`.
2. Check job output for the specific host.
3. Check Capsule logs for SSH connection attempts.
4. Verify SSH port and host key trust.

## Safety

- REX executes commands on managed hosts — HIGH RISK.
- Always verify the job template content before launching.
- Prefer read-only commands first (check mode).
- Never run destructive commands via REX without explicit approval.
