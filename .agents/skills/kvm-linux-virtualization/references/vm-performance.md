# VM Performance — KVM Virtualization

## Key Metrics

| Metric | Check | Tool |
|--------|-------|------|
| CPU steal | High steal = host contention | `top` in guest |
| vCPU overcommit | Total vCPU > physical cores | `virsh vcpucount`, `nproc` |
| NUMA alignment | Cross-NUMA access is slow | `numactl --hardware`, `virsh numatune` |
| Memory pressure | Swapping in host or guest | `free -h`, `vmstat` |
| Hugepages | Not configured or exhausted | `cat /proc/meminfo | grep Huge` |
| Disk latency | High await or I/O wait | `iostat -x`, `iotop` |
| Snapshot depth | Deep chains slow disk I/O | `qemu-img info --backing-chain` |
| Network drops | Packet loss, high latency | `ip -s link`, `ping` |

## Distinguishing Host vs Guest Issues

- **High steal time in guest** → Host contention (CPU overcommit).
- **High I/O wait in guest, low in host** → Guest misconfiguration.
- **High I/O wait in both** → Storage backend bottleneck.
- **Network slow in guest only** → Virtio config, MTU, bridge issue.

## Optimization Recommendations

Do NOT recommend any of these without workload evidence:
- CPU pinning.
- Hugepages.
- NUMA pinning.
- Disabling KSM.
- Changing I/O scheduler.

Always measure BEFORE and AFTER any change.
