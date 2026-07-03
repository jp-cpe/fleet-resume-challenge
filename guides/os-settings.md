# OS settings — step-by-step

> Step-by-step companion to [Module 9: OS settings](../modules/9-os-settings.md). Methods change fast; if anything here is out of date, defer to the linked Fleet docs.

## Custom OS settings
**Goal:** Deploy platform-specific configuration profiles and confirm they apply on target hosts.

> Documentation: [Configuration profiles (custom OS settings)](https://fleetdm.com/guides/custom-os-settings)

### macOS
1. Create or obtain a `.mobileconfig` file — e.g., a screensaver lock delay, Wi-Fi restriction, or dock configuration.
2. Controls → OS settings → Custom settings → Add profile → upload the `.mobileconfig`.
3. Target your macOS test host or fleet and save.
4. On the host's **Host details** page, confirm the profile appears under OS settings with a status of applied.

### Windows
1. Create or obtain a Windows CSP (Configuration Service Provider) profile — e.g., a BitLocker or Windows Update setting.
2. Controls → OS settings → Custom settings → Add profile → upload the CSP.
3. Target your Windows test host or fleet and save.
4. On the host's **Host details** page, confirm the setting appears under OS settings.

### Verify
1. Open **Host details** for each target host and locate the OS settings section.
2. Confirm each profile shows as applied (not pending or failed).
3. **Done when:** a custom configuration profile is deployed and applied on at least one macOS host, and a custom OS setting is deployed and applied on at least one Windows host.

> **Tip:** You can also manage profiles via GitOps in `lib/profiles/` once you're comfortable with the UI workflow. GitOps is the recommended path for production configs.
