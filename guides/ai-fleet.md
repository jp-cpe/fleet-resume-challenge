# AI + Fleet — step-by-step

> Step-by-step companion to [Bonus 4: AI + Fleet](../modules/bonus-4-ai-fleet.md). Methods change fast; if anything here is out of date, defer to the linked Fleet docs.

## AI-assisted osquery query
**Goal:** Use AI to draft a query, validate it against Fleet docs, and deploy it.

1. Pick a practical question about your fleet — e.g., "Which hosts have Chrome installed?" or "Which Macs don't have FileVault enabled?"
2. Prompt an AI assistant (Claude, ChatGPT, etc.) with context: you're using Fleet + osquery, and you need a query that works on macOS, Windows, and Linux.
3. **Validate before deploying:** cross-check the AI's output against:
   - [osquery table schema](https://fleetdm.com/tables) — confirm the tables and columns exist
   - [Fleet query docs](https://fleetdm.com/docs/using-fleet/understanding-host-vitals) — confirm the query syntax is correct
4. Run the query as a **live query** in Fleet to confirm results look right.
5. If satisfied, add the query to `lib/queries/` in your GitOps repo and sync.

## AI-assisted GitOps config
**Goal:** Use AI to generate a GitOps snippet and verify it works in your sandbox.

1. Pick a config you need — e.g., a new policy, scheduled query, or software package entry.
2. Prompt the AI with your existing GitOps repo structure and the [Fleet YAML reference](https://fleetdm.com/docs/configuration/yaml-files) as context.
3. **Validate before deploying:** compare the generated YAML against the official docs — check field names, required keys, and platform filters.
4. Add the snippet to your GitOps repo, open a PR, and confirm the dry-run passes.
5. Merge and confirm the config appears in Fleet after sync.

## End-to-end workflow
**Goal:** Demonstrate one practical AI + Fleet workflow from prompt to production.

1. Choose a real task from your sandbox (e.g., "create a policy that checks for automatic updates on Linux").
2. Use AI to draft the query and GitOps YAML.
3. Validate against docs, deploy via GitOps, and confirm it works in Fleet.
4. **Done when:** at least one AI-assisted query or GitOps config has been validated against official docs and deployed to your Fleet instance.

> **Tip:** Always treat AI output as a draft. Fleet's docs and your live sandbox are the source of truth — AI gets you 80% there fast, but validation catches the rest.
