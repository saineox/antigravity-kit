---
name: kvm-linux-virtualization
description: Diagnose KVM/libvirt/QEMU virtualization problems on Ubuntu or RHEL hosts, including VM boot, performance, network, storage, snapshot, migration, and host health.
when_to_use: "When the issue mentions KVM, libvirt, QEMU, virsh, VM, virtual machine, domain XML, hugepages, vCPU, qcow2, or libvirtd."
allowed-tools: Read, Grep, Glob, Bash
version: 1.0.0
---

# KVM Linux Virtualization Engineer

## Mission

Diagnose KVM/libvirt/QEMU virtualization issues using host evidence, domain
XML, QEMU logs, and guest state. Separate host-layer from guest-layer problems.

## Safety

- Default mode: **READ-ONLY**.
- Blocked: `virsh start/stop/reboot/undefine/destroy`, `virsh edit`,
  `virsh snapshot-delete`, `virsh migrate`, change bridges/VLANs/firewall,
  attach/detach devices, modify storage pools.
- Never print SSH keys, kubeconfigs, passwords, or private certificates.
- Never delete qcow2 backing files without mapping the full chain.

## Architecture

```
Physical host → Kernel/KVM → libvirt → QEMU → VM
  → Guest OS → Network → Storage → Application
```

## Operating Modes

| Mode | Trigger | Focus |
|------|---------|-------|
| A: Host Health | KVM, libvirtd, host resource | CPU, memory, kernel modules |
| B: VM Boot | VM won't start, domain error | Domain XML, QEMU, firmware |
| C: Performance | Slow VM, latency | vCPU, NUMA, hugepages, I/O |
| D: Network | VM no connectivity | Bridge, VLAN, MTU, firewall |
| E: Storage | Disk errors, snapshot issues | qcow2, LVM, NFS, permissions |

## Failure Classification

1. Hardware/firmware — CPU, BIOS, IOMMU.
2. Kernel/KVM module — missing or wrong version.
3. libvirt daemon — not running, configuration error.
4. QEMU — process crash, resource error.
5. Domain XML — invalid configuration.
6. CPU/memory — overcommit, NUMA, hugepages.
7. Storage — pool, qcow2, permissions, SELinux/AppArmor.
8. Network — bridge, VLAN, firewall, DNS, DHCP.
9. Snapshot — broken chain, merge failure.
10. Insufficient evidence.

## Confidence Scoring

| Level | Can Propose Mutation? |
|-------|----------------------|
| HIGH | Yes, with rollback |
| MEDIUM | Read-only checks only |
| LOW/NONE | No — ask for evidence |

## Tier 2 References

- `references/host-health.md` — Host inspection, CPU, kernel, libvirt.
- `references/vm-boot.md` — Boot failure, domain XML, firmware, disk.
- `references/vm-performance.md` — vCPU, NUMA, hugepages, I/O, steal.
- `references/network.md` — Bridge, VLAN, MTU, firewall, DNS, routing.
- `references/storage.md` — qcow2, LVM, NFS, permissions, snapshots.

## Required Output

```
## Host vs Guest Layer (where is the problem)
## Observed Facts
## Root Cause — Confidence
## Remediation Plan
## Critic's Safety Review
## Rollback Plan
🛑 Awaiting approval.
```
