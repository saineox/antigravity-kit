# Storage — KVM Virtualization

## Storage Path

```
Guest filesystem → Virtual disk → QEMU block device → libvirt storage pool
  → host filesystem/LVM/NFS/iSCSI/Ceph → physical storage
```

## Investigation

1. Disk visibility: `virsh domblklist <vm>`.
2. Storage pool: `virsh pool-list --all`, `virsh pool-info <pool>`.
3. Disk format: `qemu-img info <disk>`.
4. Snapshot chain: `qemu-img info --backing-chain <disk>`.
5. Disk capacity: `du -sh <disk>`, `df -h` on host path.
6. Thin provisioning: actual size vs virtual size.
7. Permissions: file owner, group, mode.
8. SELinux: `ls -lZ <disk>` on RHEL.
9. AppArmor: check `/var/log/syslog` for denials on Ubuntu.
10. I/O latency: `iostat -x` on the host storage device.

## Common Failures

| Symptom | Cause |
|---------|-------|
| "Cannot access storage" | Pool inactive, file deleted, permissions |
| Read-only filesystem in guest | Filesystem errors, disk corruption |
| Slow I/O | Snapshot chain too deep, storage backend slow |
| Disk full | Thin provisioning exhausted, host filesystem full |
| Lock error | Another process holds the disk lock |

## Safety

- NEVER delete qcow2 files without mapping the full backing chain.
- NEVER merge snapshots without understanding the chain order.
- ALWAYS verify the backing chain before any snapshot operation.
- Pool deactivation can make all VMs using it fail to start.
