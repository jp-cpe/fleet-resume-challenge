# 10. Disk encryption

Enforce disk encryption (FileVault on macOS, BitLocker on Windows, LUKS2 on Linux) and escrow recovery keys in Fleet.

> **Requires Fleet Premium** — free to complete with a Fleet Premium trial (see [Module 1](1-deploy-fleet.md)).

## Goal
Fleet-managed disk encryption enabled on macOS and Windows, with recovery keys escrowed and visible in host details.

## Resources
- [Step-by-step: Disk encryption](../guides/disk-encryption.md)
- [Enforce disk encryption](https://fleetdm.com/guides/enforce-disk-encryption)

## Tasks
- [ ] Enable Fleet-managed disk encryption for macOS (FileVault) via GitOps
- [ ] Enable Fleet-managed disk encryption for Windows (BitLocker) via GitOps
- [ ] Confirm recovery keys appear escrowed in Fleet on the Host details page for each host

## Done when
- [ ] FileVault is enabled and a recovery key is escrowed in Fleet for at least one macOS host
- [ ] BitLocker is enabled and a recovery key is escrowed in Fleet for at least one Windows host
