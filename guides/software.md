# Software — step-by-step

> Step-by-step companion to [Module 7: Software](../modules/7-software.md). Methods change fast; if anything here is out of date, defer to the linked Fleet docs.

## Software delivery
**Goal:** Hit every software path.

1. **Install on enroll**: add a package via GitOps to Workstations — e.g., Chrome `.pkg`. In `lib/software/`, set `url`, `hash_sha256`, `install_during_setup: true`.
2. **Self-service**: second package (Slack/Zoom) with `self_service: true`. On your Mac, open Fleet Desktop → Self-service → install.
3. **Fleet-maintained app (FMA)**: add a maintained app like 1Password — these auto-update.
4. **VPP / App Store**: if you have an Apple Business Manager sandbox, connect VPP (Settings → Integrations → MDM → Volume Purchasing) and deploy a free App Store app.
5. **Uninstall**: trigger an uninstall on one host, confirm success.
6. **Inventory + vulns**: Software page should show inventory and CVEs (Premium).
7. **Done when:** ≥2 packages deployed, 1 self-service install completed from an end-user device, vuln data visible.
