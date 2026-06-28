# Conditional access — step-by-step

> Step-by-step companion to [Bonus 3: Conditional access](../modules/bonus-3-conditional-access.md). Methods change fast; if anything here is out of date, defer to the linked Fleet docs.

## Conditional access
**Goal:** Walk through the "Fleet as a trust signal" story.

1. Settings → Integrations → Conditional access. Connect Microsoft Entra ID (free tenant) or Okta.
2. Define a "compliant" rule: must pass your chosen compliance policies.
3. Configure Entra/Okta to require Fleet compliance for one test SaaS app.
4. Test: compliant host signs in (works). Force a policy failure, retry (blocked).
