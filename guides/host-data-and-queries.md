# Host data & queries — step-by-step

> Step-by-step companion to [Module 5: Host data & queries](../modules/5-host-data-and-queries.md). Methods change fast; if anything here is out of date, defer to the linked Fleet docs.

## Host vitals

**Goal:** Understand the data Fleet collects automatically.

Host vitals are the built-in, osquery-powered details Fleet refetches from every host on a schedule — OS version, hardware/serial, disk-encryption status, installed software inventory, battery health, network interfaces, MDM enrollment status, and more. They populate the **Host details** page, and you don't write any queries to get them.

1. Open a host's **Host details** page and explore the vitals Fleet collected (OS, hardware, software inventory, disk encryption, etc.).
2. Trigger a **Refetch** and watch the data update.
3. **Done when:** you can point to where Fleet surfaces disk encryption status and software inventory on a host, and explain that these come from Fleet's built-in queries.



## Reports (custom osquery)

**Goal:** Test every report modality.

1. **Scheduled report**: [Add a report to Fleet using GitOps](https://fleetdm.com/docs/configuration/yaml-files#reports), include an `interval`, and confirm it appears in the UI after sync.
2. Add a second report with `interval`=`0`.
3. **Live report** (ad-hoc): Run the second report live against one of your hosts. See: [Run a live report using fleetctl](https://fleetdm.com/guides/get-current-telemetry-from-your-devices-with-live-queries#running-a-live-report-using-fleetctl), [run a report using the REST API](https://fleetdm.com/docs/rest-api/rest-api#run-live-report), or the [Fleet API Postman collection](https://github.com/jp-cpe/fleet-api-postman-collection).
4. **Done when:** 1 scheduled query collecting (configured via GitOps), 1 live report run via `fleetctl` or the REST API.

