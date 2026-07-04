# Policies — step-by-step

> Step-by-step companion to [Module 6: Policies](../modules/6-policies.md). Methods change fast; if anything here is out of date, defer to the linked Fleet docs.

## Policies and remediation

**Goal:** Pass/fail compliance with GitOps-managed auto-remediation.

1. Create 2-3 cross-platform policies in your GitOps repo:
  - Example 1: Check whether all available software updates are installed on macOS hosts
  - Example 2: Check whether disk encryption is enabled on Linux hosts
  - Example 3: Check whether Secure Boot is enabled on Windows hosts
2. On one policy, add an automation using `run_script`
  > Reference a script from your repo that the policy will run on failure.
3. Configure `failing_policies_webhook` in your team YAML to send failures to `webhook.site` for verification. Later you'll swap this to Slack/Tines.
4. Force a failure, confirm remediation script runs and webhook fires. Watch the policy status change in the UI.
  > Hint: `query: SELECT 1 WHERE 1=0` will always fail.

   > **Why a policy can fail but not fire the webhook:** Fleet doesn't turn a failing policy in the UI into a webhook immediately. It handles those two steps on different schedules:
   >
   > 1. **Policy runs** run on a schedule (~hourly). When a policy fails there, the host shows up as failing in the UI.
   > 2. **The automation job** runs on its own schedule (default roughly once per 24 hours). During that job, Fleet checks for newly failing policies and sends the configured webhooks.
   >
   > So in practice:
   > - Seeing a policy go red/failing in the UI does **not** mean the webhook has fired yet.
   > - Live policy runs do **not** trigger webhooks; only scheduled runs count.
   > - See the [Fleet automations docs](https://fleetdm.com/docs/using-fleet/automations).
   >
   > **To force it now:**
   > ```bash
   > fleetctl trigger --name cleanups_then_aggregation
   > fleetctl trigger --name automations
   > ```
5. Take note of all available automations for policies: create ticket, send webhook, calendar event, conditional access, install software, and the option to do continuous automations. [Learn more](https://fleetdm.com/guides/automations)
6. **Done when:** 2-3 policies across different platforms (all in GitOps YAML), 1 with working auto-remediation (`run_script`), and a webhook firing on policy failure.

## Scripts and host actions

**Goal:** Scripts as a primary remediation/automation surface.

1. Add 3 more scripts to your repo:
  - macOS zsh: `system_profiler SPHardwareDataType`
  - Windows PowerShell: `Get-ComputerInfo`
  - Linux bash: `uname -a; lsb_release -a`
2. Reference them under `scripts:` in team yml.
3. **Run a script using `fleetctl`**. Confirm output captured.
4. **Run a script using the REST API**. Confirm output captured.
5. Verify the policy-failure script from before ran automatically.
6. **Done when:** 1 script run via `fleetctl`, 1 script run via the REST API, 1 script runs from a policy.

