# Webhooks — step-by-step

> Step-by-step companion to [Bonus 2: Webhooks](../modules/bonus-2-webhooks.md). Methods change fast; if anything here is out of date, defer to the linked Fleet docs.

## Slack integration
**Goal:** Human-in-the-loop notifications.

1. Create an incoming webhook in your Slack workspace for `#fleet-sandbox`.
2. Settings → Integrations → Automations. Wire up 3 automations to Slack:
   - Policy failure (FileVault policy)
   - Failing host count threshold
   - Vuln automation: CVSS ≥ 7 on any software
3. Trigger each, confirm messages arrive.
4. **Bonus:** build a small Slack bot (Bolt JS) that listens for `/fleet hosts` and calls the Fleet API. Host on Render free tier.

## Tines workflow
**Goal:** Fleet → SOAR orchestration. Classic CSA demo.

1. Sign up for Tines Community Edition (free).
2. Build a story that:
   - Receives a webhook from Fleet on policy failure
   - Parses host + policy name
   - Looks up host owner via `GET /api/v1/fleet/hosts/:id`
   - Posts to Slack tagging the owner
   - Creates a Jira/Linear ticket (or just logs to a Tines event)
3. Repoint the policy-failure webhook from `webhook.site` to Tines.
4. Force a failure, watch the full chain.
