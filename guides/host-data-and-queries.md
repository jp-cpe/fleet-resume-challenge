# Host data & queries — step-by-step

> Step-by-step companion to [Module 5: Host data & queries](../modules/5-host-data-and-queries.md). Methods change fast; if anything here is out of date, defer to the linked Fleet docs.

## Host vitals
**Goal:** Understand the data Fleet collects automatically.

Host vitals are the built-in, osquery-powered details Fleet refetches from every host on a schedule — OS version, hardware/serial, disk-encryption status, installed software inventory, battery health, network interfaces, MDM enrollment status, and more. They populate the **Host details** page, and you don't write any queries to get them.

1. Open a host's **Host details** page and explore the vitals Fleet collected (OS, hardware, software inventory, disk encryption, etc.).
2. Trigger a **Refetch** and watch the data update.
3. **Done when:** you can point to where Fleet surfaces disk encryption status and software inventory on a host, and explain that these come from Fleet's built-in queries.

## Reports (custom osquery)
**Goal:** Demo and troubleshoot every report modality via GitOps.

1. **Live report** (ad-hoc): Run a live report from the **Queries** page using `fleetctl` or the REST API: [how to run a live report](https://fleetdm.com/guides/get-current-telemetry-from-your-devices-with-live-queries#basic-article).
2. **Scheduled report** (recurring → log destination via GitOps): Add to `lib/queries/scheduled.yml`:
   ```yaml
   - name: osquery_info_hourly
     query: SELECT * FROM osquery_info;
     interval: 3600
     platforms:
       - darwin
       - linux
       - windows
     snapshot: false
   ```
   Then reference it in your team YAML under `scheduled_queries:`. Confirm it appears in **Queries → Scheduled** after the GitOps sync.
3. Save 1 custom query in `lib/queries/` in your GitOps repo, confirm it appears in the UI after sync.
4. **Done when:** 1 live query run from the UI, 1 scheduled query collecting (configured via GitOps), 1 custom query added via GitOps.
