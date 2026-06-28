# SIEM | logging — step-by-step

> Step-by-step companion to [Bonus 1: SIEM | logging](../modules/bonus-1-siem.md). Methods change fast; if anything here is out of date, defer to the linked Fleet docs.

## SIEM (log destinations)
**Goal:** Every query/policy/script result can ship to a SIEM.

1. Pick a free-tier SIEM: Elastic Cloud trial, Splunk Free, or Grafana Cloud + Loki.
2. In Fleet (env vars on Render): set `FLEET_OSQUERY_RESULT_LOG_PLUGIN` and `FLEET_OSQUERY_STATUS_LOG_PLUGIN`. Easiest for a sandbox is `webhook` pointing at a Loki/Logstash HTTP input. Other options: firehose, kinesis, kafka, pubsub, filesystem.
3. Schedule a noisy query (`SELECT * FROM processes LIMIT 50;` every 5 min on Workstations).
4. Confirm results land in the SIEM and are searchable.
5. Build 1 dashboard panel (e.g., "top processes by host").
