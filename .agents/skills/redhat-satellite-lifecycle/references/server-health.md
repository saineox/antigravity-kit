# Server Health — Red Hat Satellite

## Satellite Services

Check all services: `satellite-maintain service status` or `foreman-maintain`.

Critical services:
- `foreman` (Satellite application).
- `postgresql` (database).
- `pulpcore-*` (content management).
- `redis` (caching).
- `dynflow-sidekiq` (task queue).

## Common Health Issues

| Symptom | Check |
|---------|-------|
| Slow UI/API | PostgreSQL connections, Pulp tasks queued, disk I/O |
| Tasks stuck | `foreman-rake foreman_tasks:cleanup` (read-only investigation first) |
| Sync failures | Pulp worker health, disk space, upstream connectivity |
| UI 500 errors | Foreman logs: `/var/log/foreman/production.log` |
| Certificate errors | Check cert expiry, CA chain, Capsule certs |

## Disk Space

Satellite requires significant disk space:
- `/var/lib/pulp` — repository content.
- `/var/lib/pgsql` — database.
- `/var/log` — logs.
Monitor all three independently.

## Backup

- `satellite-maintain backup offline` (full backup, downtime required).
- `satellite-maintain backup online` (incremental, minimal impact).
- Always verify backup integrity before any major change.

## Evidence

```bash
satellite-maintain service status
satellite-maintain health check
df -h /var/lib/pulp /var/lib/pgsql /var/log
tail -50 /var/log/foreman/production.log
```
