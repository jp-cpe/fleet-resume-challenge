# 21. Conditional access

Use Fleet as a trust signal.

> 🏗️  Keep your hard hat on, this content is under construction.

## Phase 17 — Conditional access
**Goal:** Walk through the "Fleet as a trust signal" story.

1. Settings → Integrations → Conditional access. Connect Microsoft Entra ID (free tenant) or Okta.
2. Define a "compliant" rule: must pass policies X, Y, Z from Phase 7.
3. Configure Entra/Okta to require Fleet compliance for one test SaaS app.
4. Test: compliant host signs in (works). Force a policy failure, retry (blocked).