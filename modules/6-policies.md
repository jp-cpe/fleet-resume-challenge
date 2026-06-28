# 6. Policies

Configure pass/fail policies in Fleet with auto-remediation.

> 🏗️  Keep your hard hat on, this content is under construction. 

## Phase 1 — Policies and remediation
**Goal:** Pass/fail compliance with optional auto-remediation.

1. Create 3 cross-platform policies:
   - **macOS** — FileVault enabled: `SELECT 1 FROM disk_encryption WHERE filevault_status = 'on' LIMIT 1;`
   - **Windows** — BitLocker enabled (use the policy library)
   - **Linux** — automatic updates enabled
2. On one policy, enable **Run script on failure** (Policies → policy → Automation). Wire up a simple remediation script.
3. Settings → Integrations → Automations → Policy automations → point a **webhook** at `webhook.site` first to verify payload shape. You'll swap to Slack/Tines later.
4. Force a failure (disable FileVault on a test Mac), confirm remediation script runs and webhook fires.
5. **Done when:** 3 policies across 3 platforms, 1 with auto-remediation, webhook firing end-to-end.

## Phase 2 — Scripts and host actions
**Goal:** Scripts as a primary remediation/automation surface.

1. Save 3 scripts in `lib/scripts/`:
   - macOS zsh: `system_profiler SPHardwareDataType`
   - Windows PowerShell: `Get-ComputerInfo`
   - Linux bash: `uname -a; lsb_release -a`
2. Reference them under `scripts:` in team yml.
3. Hosts page → host → Actions → **Run script**. Confirm output captured.
4. Verify the policy-failure script from before ran automatically.
5. **Done when:** 3 scripts run via UI, 1 runs from a policy.