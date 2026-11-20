# VM Boot Failure — KVM Virtualization

## Investigation Order

```
Domain state → QEMU process → Console → Domain XML → Boot disk
  → Firmware → Snapshot chain → SELinux/AppArmor → CPU model → Memory
```

## Checks

1. Domain state: `virsh domstate <vm>`.
2. QEMU process: `ps aux | grep qemu | grep <vm>`.
3. Console output: `virsh console <vm>` (read-only observation).
4. Domain XML: `virsh dumpxml <vm>` — check disk, network, firmware.
5. Boot disk availability: verify path exists and is readable.
6. Firmware: BIOS vs UEFI, check OVMF/AAVMF paths.
7. Boot order: check `<boot dev>` in domain XML.
8. Snapshot chain: `qemu-img info --backing-chain <disk>`.
9. SELinux/AppArmor: check denial logs.
10. CPU model: check compatibility with host CPU.
11. Memory: check allocation vs available.

## Common Failures

| Error | Cause |
|-------|-------|
| "Cannot access storage" | Disk moved, permissions, pool inactive |
| "Cannot allocate memory" | Host memory exhausted |
| "Unsupported CPU" | CPU model not supported by host |
| "No bootable device" | Boot order wrong, disk empty, ISO removed |
| "Permission denied" | SELinux/AppArmor blocking QEMU |

## Safety

Do NOT immediately recreate the VM. Diagnose the root cause first.
