# Disk encryption — step-by-step

> Step-by-step companion to [Module 10: Disk encryption](../modules/10-disk-encryption.md). Methods change fast; if anything here is out of date, defer to the linked Fleet docs.

## Fleet-managed disk encryption
**Goal:** Enable FileVault and BitLocker through Fleet and escrow recovery keys.

> Documentation: [Enforce disk encryption](https://fleetdm.com/guides/enforce-disk-encryption)

### macOS (FileVault)
1. Controls → Disk encryption → macOS → enable Fleet-managed FileVault.
2. Select the fleet(s) or hosts to target and save.
3. On your test Mac, confirm FileVault begins enabling (you may need to log out or restart).
4. Open the host's **Host details** page → Disk encryption section → confirm a recovery key is escrowed in Fleet.

### Windows (BitLocker)
1. Controls → Disk encryption → Windows → enable Fleet-managed BitLocker.
2. Select the fleet(s) or hosts to target and save.
3. On your test Windows host, confirm BitLocker begins enabling.
4. Open the host's **Host details** page → Disk encryption section → confirm a recovery key is escrowed in Fleet.

### Verify escrow
1. Hosts → select each encrypted host → **Host details**.
2. Locate the disk encryption section and confirm the recovery key is stored in Fleet (not just locally on the device).
3. **Done when:** FileVault is enabled with an escrowed recovery key on at least one macOS host, and BitLocker is enabled with an escrowed recovery key on at least one Windows host.

> **Note:** Linux LUKS2 encryption is also supported. If you have a Linux host in your sandbox, enable it via Controls → Disk encryption → Linux.
