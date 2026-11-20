---
name: linux-virtualization-correlator
description: Cross-layer incident correlator for KVM host ↔ RHEL guest ↔ Red Hat Satellite problems.
when_to_use: "When a VM is healthy but Satellite cannot manage it, or when a Satellite operation causes a guest or KVM issue, or when the problem spans hypervisor, guest OS, and content management layers."
allowed-tools: Read, Grep, Glob, Bash
version: 1.0.0
---

# Linux Virtualization Incident Correlator

## Mission

Correlate failures that span the KVM host, RHEL guest, and Red Hat Satellite
layers. Loaded ONLY when the linux-infra-sre agent detects the incident
crosses 2+ layers.

## Layer Correlation Map

```
Ubuntu KVM host → libvirt/QEMU → Virtual network/storage
  → RHEL guest → Guest OS/Network → Satellite registration
  → Content View → Repository → Errata → Remote Execution
```

## Cross-Layer Failure Patterns

| Symptom | Layers Involved | Investigation |
|---------|----------------|---------------|
| VM healthy, Satellite can't register | Guest + Satellite | DNS, time, certs, Capsule path |
| Satellite REX fails, VM accessible via SSH | Satellite + Guest | Capsule SSH config, sudo, SELinux |
| VM rebooted, lost Satellite registration | KVM + Guest + Satellite | Consumer identity, hostname change |
| Patch applied via Satellite, VM won't boot | Satellite + Guest + KVM | Kernel update broke boot, GRUB |
| Network change on host, all guest Satellite broken | KVM + Guest + Satellite | Bridge/VLAN change broke connectivity |
| Storage full on host, Satellite sync fails | KVM + Satellite | Same storage backend for VM + Satellite |

## Correlation Protocol

1. Identify which layer the user reports the problem in.
2. Check the layer BELOW (infrastructure) for root cause.
3. Check the layer ABOVE (management) for cascade effects.
4. Map timeline: what changed, when, in which layer.
5. The root cause is usually in the LOWEST failing layer.

## Safety

- This skill is READ-ONLY correlation only.
- It identifies which domain skill should handle the fix.
- It does not propose mutations.
