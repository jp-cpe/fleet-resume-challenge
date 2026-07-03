# MDM — step-by-step

> Step-by-step companion to [Module 8: MDM](../modules/8-mdm.md). Methods change fast; if anything here is out of date, defer to the linked Fleet docs.

## MDM setup
**Goal:** Configure the MDM infrastructure Fleet needs to manage Apple and Windows devices.

> Documentation: [MDM macOS setup](https://fleetdm.com/docs/using-fleet/mdm-macos-setup) | [MDM Windows setup](https://fleetdm.com/docs/using-fleet/mdm-windows-setup)

1. **Apple Push Notification service (APNs):** Settings → Integrations → MDM → Apple MDM → upload your APNs certificate. Fleet uses this to send MDM commands to macOS and iOS devices.
2. **Apple Business Manager (ABM) and Automated Device Enrollment (ADE):** Connect your ABM account so Fleet can enroll devices at setup time. Upload your server token and configure your default ADE team.
3. **SCEP:** Configure a SCEP server (or use Fleet's built-in SCEP proxy) so devices can request identity certificates during enrollment.
4. **Windows MDM:** If you have Windows hosts, complete the Windows MDM enrollment flow per the Windows setup guide.
5. **Done when:** MDM integrations show as connected in Fleet and at least one host is MDM-enrolled.

## Deploy a configuration profile
**Goal:** Push an OS setting to a test host and confirm it applied.

1. Controls → OS settings → Custom settings → Add profile.
2. Upload a simple `.mobileconfig` (macOS) or CSP (Windows) — e.g., a Wi-Fi restriction or screensaver timeout profile.
3. Target your test host or fleet and save.
4. On the host's **Host details** page, confirm the profile appears under OS settings.
5. **Done when:** a configuration profile is deployed and applied to at least one enrolled host.

## Issue an MDM command
**Goal:** Send a remote command and confirm the host acknowledges it.

1. Issue one command via the REST API — **Restart**, **Lock** (Premium), or **Wipe** (Premium). Restart is the safest choice for a sandbox. See the [Fleet API Postman collection](https://github.com/jp-cpe/fleet-api-postman-collection).
2. Confirm the command status shows as acknowledged on the host's details page.
3. **Done when:** at least one MDM command has been issued and acknowledged.
