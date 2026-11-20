# Network — KVM Virtualization

## Network Path

```
Guest interface → Virtio device → QEMU backend → libvirt network/bridge
  → VLAN/bond → host NIC → upstream network → DNS/gateway/endpoint
```

## Investigation

1. Guest interface state: `ip link`, `ip addr` inside guest.
2. MAC address: match domain XML `<mac>` with guest `ip link`.
3. Bridge membership: `brctl show` or `ip link show master <bridge>`.
4. VLAN: check VLAN tag in domain XML and host.
5. MTU: mismatch causes silent drops — check guest, bridge, and host.
6. Firewall: `iptables -L -n` (host), `firewall-cmd` (guest/host).
7. DNS: check `/etc/resolv.conf` in guest.
8. DHCP: check dnsmasq logs for libvirt NAT networks.
9. NetworkManager vs Netplan: Ubuntu uses Netplan, RHEL uses NM.

## Common Failures

| Symptom | Cause |
|---------|-------|
| No connectivity | Bridge not connected, firewall blocking |
| Intermittent drops | MTU mismatch, duplex mismatch |
| Cannot reach external | NAT misconfigured, default route missing |
| Cannot reach other VMs | Same bridge? Same VLAN? |
| DNS failure | Resolver not configured, dnsmasq not running |

## Safety

Never change bridge, VLAN, or firewall without explicit approval.
Network changes can disconnect all VMs on the affected bridge.
