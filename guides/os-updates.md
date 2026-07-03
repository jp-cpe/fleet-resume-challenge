# OS updates — step-by-step

> Step-by-step companion to [Module 11: OS updates](../modules/11-os-updates.md). Methods change fast; if anything here is out of date, defer to the linked Fleet docs.

## Minimum OS version enforcement
**Goal:** Set minimum OS version requirements and confirm enforcement status on enrolled hosts.

> Documentation: [Enforce OS updates](https://fleetdm.com/guides/enforce-os-updates)

### macOS
1. Controls → OS updates → macOS.
2. Set a minimum macOS version (e.g., one version behind your test Mac's current OS).
3. Choose enforcement behavior (nudge, force, or deadline-based) and save.
4. Confirm your test Mac shows OS update enforcement status on its **Host details** page.

### Windows
1. Controls → OS updates → Windows.
2. Set a minimum Windows version for your enrolled Windows hosts.
3. Choose enforcement behavior and save.
4. Confirm your test Windows host shows OS update enforcement status on its **Host details** page.

### Verify
1. Hosts → select a host → **Host details** → OS updates section.
2. Confirm the host reflects the minimum version requirement and current compliance status.
3. **Done when:** minimum OS version requirements are set for at least two platforms, and at least one host shows OS update enforcement status.

> **Optional:** If you have iOS or iPadOS devices enrolled, configure minimum version enforcement for those platforms as well via Controls → OS updates.
