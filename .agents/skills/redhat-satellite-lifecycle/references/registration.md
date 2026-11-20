# Registration — Red Hat Satellite

## Pre-Registration Checklist

1. Guest hostname and FQDN set correctly.
2. DNS resolution: guest can resolve Satellite/Capsule FQDN.
3. Time synchronized (chrony/ntp) — certificate validation fails with time drift.
4. Satellite/Capsule reachable on required ports (443, 5647, 8000, 8140, 9090).
5. CA certificate installed and trusted on the guest.

## Registration Methods

- `subscription-manager register --org --activationkey` (preferred).
- `curl -s https://satellite.example.com/register | bash` (bootstrap).
- Both require valid Activation Key, Organization, and Location.

## Common Failures

| Symptom | Cause |
|---------|-------|
| "Unable to connect" | DNS, firewall, proxy, Satellite down |
| "Certificate verify failed" | CA not installed, time drift, wrong FQDN |
| "Invalid activation key" | Wrong key name, wrong org, key disabled |
| "This system is already registered" | Duplicate consumer identity |
| "No subscriptions available" | Manifest exhausted, wrong org |

## Distinguishing Failure Layers

- Network failure → Can't reach Satellite at all.
- Certificate failure → Reaches Satellite but TLS handshake fails.
- Auth failure → TLS OK but credentials rejected.
- Org/Location → Registered but wrong content attached.

## Evidence

```bash
hammer host list --organization "ORG"
hammer activation-key list --organization "ORG"
subscription-manager status
subscription-manager repos --list-enabled
```
