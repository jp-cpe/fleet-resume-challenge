# Policies — step-by-step

> Step-by-step companion to [Module 6: Policies](../modules/6-policies.md). Methods change fast; if anything here is out of date, defer to the linked Fleet docs.

## Policies and remediation
**Goal:** Pass/fail compliance with GitOps-managed auto-remediation.

1. Create 3 cross-platform policies in `lib/policies/` in your GitOps repo:
   ```yaml
   - name: FileVault enabled (macOS)
     platforms: [darwin]
     query: SELECT 1 FROM disk_encryption WHERE filevault_status = 'on' LIMIT 1;
   - name: BitLocker enabled (Windows)
     platforms: [windows]
     query: SELECT 1 FROM bitlocker_status WHERE protection_status = 1 LIMIT 1;
   - name: Automatic updates enabled (Linux)
     platforms: [linux]
     query: SELECT 1 FROM security_patches WHERE security_patch = 1 LIMIT 1;
   ```
2. On one policy (FileVault), add auto-remediation via GitOps:
   ```yaml
   - name: FileVault enabled (macOS)
     platforms: [darwin]
     query: SELECT 1 FROM disk_encryption WHERE filevault_status = 'on' LIMIT 1;
     remediation: enable-filevault
   ```
   Reference a script from `lib/scripts/enable-filevault` that the policy will run on failure.
3. Configure a policy automation webhook in your team YAML to send failures to `webhook.site` for verification. Later you'll swap this to Slack/Tines.
4. Force a failure (disable FileVault on a test Mac), confirm remediation script runs and webhook fires.
5. **Done when:** 3 policies across 3 platforms (all in GitOps YAML), 1 with auto-remediation configured, webhook firing end-to-end.

## Scripts and host actions
**Goal:** Scripts as a primary remediation/automation surface.

1. Save 3 scripts in `lib/scripts/`:
   - macOS zsh: `system_profiler SPHardwareDataType`
   - Windows PowerShell: `Get-ComputerInfo`
   - Linux bash: `uname -a; lsb_release -a`
2. Reference them under `scripts:` in team yml.
3. Hosts page → host → Actions → **Run script**. Confirm output captured.
4. Verify the policy-failure script from before ran automatically.
5. **Done when:** 3 scripts run via UI, 1 runs from a policy.
