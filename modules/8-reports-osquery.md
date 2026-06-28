# 8. Reports | osquery

Run osquery queries using Reports in Fleet.

> 🏗️  Keep your hard hat on, this content is under construction. 

## Phase 1 — All three Report types
**Goal:** Demo and troubleshoot every report modality.

1. **Live report** (ad-hoc): Run a live report using one of the methods detailed in this guide on [how to run a live report](https://fleetdm.com/guides/get-current-telemetry-from-your-devices-with-live-queries#basic-article)
2. **Scheduled report** (recurring → log destination): `SELECT * FROM osquery_info;` hourly on Servers, log as snapshot.
3. Save 1 custom query in `lib/queries/` in your GitOps repo, confirm it appears in the UI after sync.
4. **Done when:** 1 live query run, 1 scheduled query collecting, 1 query added via GitOps (not UI).