# Fleet Resume Challenge 

**Goal:** Stand up a personal Fleet Premium sandbox and exercise at least ~80% of the product surface, so you can add Fleet skills to your resume.

**Estimated time:** 7-10 working days end-to-end, depending on how deep you go on MDM and integrations.

**Note:** This outline was originally generated using Claude. I am cleaning it up as I go, but have not finished. **Always reference the official Fleet documentation for the most up-to-date information.**

---

## Phase 1 — Deploy Fleet
**Goal:** A working Fleet instance you own end-to-end.

> Documentation: [Deploy Fleet](https://fleetdm.com/docs/deploy/deploy-fleet)

1. Follow the above documentation to deploy Fleet.

    > Render is the simplest method but costs $65/mo. Docker is another good method for free local testing.

2. You can sign up for a 3-day premium trial on the [Fleet website](https://fleetdm.com/login#).

3. Add `FLEET_LICENSE_KEY` to the web service env vars in Render or the appropriate location within your setup. Without it, you can't test any premium features. 

4. Open the public URL, complete setup (name/email/password).

5. **Done when:** UI loads, Hosts page is empty, and My account page shows a license expiration date

> If you need help obtaining a license key for your Fleet server, reach out for help in the [#fleet channel](https://macadmins.slack.com/archives/C0214NELAE7) of the Mac Admins Slack.

---

## Phase 2 — Create lab VMs with UTM
**Goal:** One disposable VM per OS so you can enroll, break, and re-enroll without touching your daily driver.

> Documentation: [UTM documentation](https://docs.getutm.app/)

1. Install [UTM](https://mac.getutm.app/) on your Mac.
2. **macOS** — Create a macOS VM (Apple Silicon) in UTM. Name it `fleet-sandbox-macos`. Complete setup with a local admin account.

   > Download an [IPSW](https://mrmacintosh.com/apple-silicon-m1-full-macos-restore-ipsw-firmware-files-database/).

3. **Windows** — Create a Windows 11 ARM VM (Apple Silicon) in UTM. Name it `fleet-sandbox-windows`. Complete setup with a local admin account.

   > Use [CrystalFetch](https://apps.apple.com/us/app/crystalfetch-iso-downloader/id6454431289?mt=12) to obtain the latest Windows installer.

4. **Linux** — Create an Ubuntu VM (ARM images on Apple Silicon). Name it `fleet-sandbox-ubuntu`. Complete setup with a local admin account.

   > Download [Ubuntu Desktop](https://ubuntu.com/download/desktop) or [Ubuntu Server](https://ubuntu.com/download/server)

5. **Networking** — Use Shared Network (NAT) in UTM so each VM has outbound internet. Confirm each guest can reach your Fleet Render URL in a browser or with `curl`.
6. Take a **snapshot** (Right-click > **Clone**) of each VM right after OS setup, before enrolling Fleet. Name them `windows-backup`, `macOS-backup`, `ubuntu-backup`, etc..
7. **Done when:** 3 VMs boot, reach the internet, and can resolve your Fleet URL.

---

## Phase 3 — Install fleetctl and enroll first hosts
**Goal:** At least one of each major OS reporting in.

> Documentation: [Fleetctl documentation](https://fleetdm.com/guides/fleetctl)

1. [Install fleetctl](https://fleetdm.com/download)
2. Review the available commands by running `fleetctl --help` in Terminal.
3. [Authenticate to your Fleet instance](https://fleetdm.com/guides/fleetctl#authentication).
4. Back in the Fleet UI: Hosts → **Add hosts** → view the fleetctl commands needed to generate Fleet's agent for each OS type.

   > Run `fleetctl package --help` in Terminal to view all the options for creating Fleet installer packages.

5. Generate installers with `fleetctl package` for macOS (`.pkg`), Windows (`.msi`), and Linux (`.deb`/`.rpm`).

   > You will need to install [Docker.app](https://docs.docker.com/desktop/setup/install/mac-install/) to create the Windows installer from a Mac.

   >  Generate the Linux agent without fleet desktop.

6. Enroll your Phase 2 VMs (macOS, Windows, and Linux) by installing each package in its guest OS.

   > Tip: Create a new directory on your computer and add it as a shared directory for each VM in the UTM settings.

   > Tip: If you have trouble accessing the shared drive on any of the VMs it may be faster to generate a new installer on the VM itself by installing fleetctl from fleetdm.com/download and running the appropriate command.

7. Optional: ChromeOS, iOS/iPadOS, and Android.
8. **Done when:** 3+ hosts show online.

---

## Phase 4 — Fleets, labels, and an API-only user
**Goal:** Understand Fleet's two grouping primitives: Fleets and Labels.

>Documentation: [Fleets](https://fleetdm.com/guides/fleets) | [Labels](https://fleetdm.com/guides/managing-labels-in-fleet)

1. Create 2 fleets via Settings → Fleets: `Workstations` and `Servers`.
2. Move Mac + Windows into Workstations, Linux into Servers.
3. Create labels via the user dropdown → Labels
   - **Dynamic** label `macOS Sonoma+`: `SELECT 1 FROM os_version WHERE platform='darwin' AND major >= 14;`
   - **Manual** label `Test devices`: add your hosts by hand.
4. Create an **API-only user** (Settings → Users → Add user → select "API-only user"). Set **Permissions** to `Global user`, **Role** to `GitOps` and **API access** to `All API endpoints`. Save the token — you'll need it for GitOps.
5. **Done when:** 2 teams populated, 1 dynamic + 1 manual label working, API token saved.

---

## Phase 5 — GitOps
**Goal:** Manage Fleet as code — and capture the config you already built in the UI instead of starting from a blank template.
>Documentation: [Fleet GitOps documentation](https://fleetdm.com/docs/configuration/yaml-files) | [Migrating to GitOps using fleetctl](https://fleetdm.com/guides/migrating-to-gitops-using-fleetctl) | [Fleet's internal GitOps repo](https://github.com/fleetdm/fleet/tree/main/it-and-security)

1. Export your current Fleet config to GitOps YAML with `fleetctl generate-gitops`. This is the migration tool that turns your existing instance (the fleets, labels, and queries from Phase 4) into best-practice GitOps files, so you don't have to hand-author them. 

   ```bash
   mkdir fleet-sandbox-gitops
   fleetctl generate-gitops --dir fleet-sandbox-gitops
   ```

   > See [Migrating to GitOps using fleetctl](https://fleetdm.com/guides/migrating-to-gitops-using-fleetctl) for the full `generate-gitops` walkthrough.

2. Explore the generated layout: `default.yml` (global), `fleets/*.yml` (per-fleet), and `lib/` (reusable software/queries/policies/profiles). Note the `# TODO` comments — `generate-gitops` redacts secrets (enroll secrets, tokens) and leaves placeholders for you to fill in.

   > You may notice that the output from `fleetctl generate-gitops` is relatively flat. If you want, you can use `fleetctl new` to generate a more comprehensive GitOps repository structure, then use AI to plug your existing configurations into the more robust directory structure.
   
   If you want to see an example of a more human-friendly structure check out [Fleet's internal GitOps repo](https://github.com/fleetdm/fleet/tree/main/it-and-security). This is the exact repository Fleet's IT team uses to manage their devices internally via GitOps.

3. Create a new GitHub repo (e.g. `fleet-sandbox-gitops`), then commit and push the generated files. Add a GitHub Actions workflow that runs `fleetctl gitops` (copy the `.github/workflows/` example from [`github.com/fleetdm/fleet-gitops`](github.com/fleetdm/fleet-gitops) if you need a reference) or use the workflows generated by `fleetctl new`.
4. In your repo → Settings → Secrets and variables → Actions, add:
   - `FLEET_URL` = your Fleet server URL
   - `FLEET_API_TOKEN` = token from Phase 4

   Depending on your configuration you may also need to add repository secrets for `FLEET_GLOBAL_ENROLL_SECRET` or other fleet enroll secrets.

6. Push a trivial change (edit `org_name` under `org_info` of your `default.yml`) → confirm the Action runs and applies it.
7. Open a PR with another change → confirm it does **dry run only** on PRs, real apply on merge to main.
8. In Fleet go to Settings → Integrations → Change management → enable **GitOps mode** to lock the UI from drift.
9. **Done when:** your live config is captured in Git, a commit to main updates Fleet within ~1 min, and GitOps mode is on.

   > NOTE: You will be operating exclusively via GitOps mode for the rest of this tutorial.

---

## Phase 6 — All three Report types
**Goal:** Demo and troubleshoot every report modality.

1. **Live report** (ad-hoc): Run a live report using one of the methods detailed in this guide on [how to run a live report](https://fleetdm.com/guides/get-current-telemetry-from-your-devices-with-live-queries#basic-article)
2. **Scheduled report** (recurring → log destination): `SELECT * FROM osquery_info;` hourly on Servers, log as snapshot.
3. **Query pack**: import a Fleet-maintained pack (e.g., `it-and-security/lib/macos/queries` from the fleet repo) via GitOps.
4. Save 1 custom query in `lib/queries/` in your GitOps repo, confirm it appears in the UI after sync.
5. **Done when:** 1 live query run, 1 scheduled query collecting, 1 query added via GitOps (not UI).

---

## Phase 7 — Policies and remediation
**Goal:** Pass/fail compliance with optional auto-remediation.

1. Create 3 cross-platform policies:
   - **macOS** — FileVault enabled: `SELECT 1 FROM disk_encryption WHERE filevault_status = 'on' LIMIT 1;`
   - **Windows** — BitLocker enabled (use the policy library)
   - **Linux** — automatic updates enabled
2. On one policy, enable **Run script on failure** (Policies → policy → Automation). Wire up a simple remediation script.
3. Settings → Integrations → Automations → Policy automations → point a **webhook** at `webhook.site` first to verify payload shape. You'll swap to Slack/Tines later.
4. Force a failure (disable FileVault on a test Mac), confirm remediation script runs and webhook fires.
5. **Done when:** 3 policies across 3 platforms, 1 with auto-remediation, webhook firing end-to-end.

---

## Phase 8 — Software delivery
**Goal:** Hit every software path.

1. **Install on enroll**: add a package via GitOps to Workstations — e.g., Chrome `.pkg`. In `lib/software/`, set `url`, `hash_sha256`, `install_during_setup: true`.
2. **Self-service**: second package (Slack/Zoom) with `self_service: true`. On your Mac, open Fleet Desktop → Self-service → install.
3. **Fleet-maintained app (FMA)**: add a maintained app like 1Password — these auto-update.
4. **VPP / App Store**: if you have an Apple Business Manager sandbox, connect VPP (Settings → Integrations → MDM → Volume Purchasing) and deploy a free App Store app.
5. **Uninstall**: trigger an uninstall on one host, confirm success.
6. **Inventory + vulns**: Software page should show inventory and CVEs (Premium).
7. **Done when:** ≥2 packages deployed, 1 self-service install completed from an end-user device, vuln data visible.

---

## Phase 9 — MDM (macOS, Windows, iOS, Android)
**Goal:** Understand MDM

1. **macOS MDM**
   - Need an Apple Developer account (and ideally a sandbox ABM).
   - Settings → Integrations → MDM → Apple → generate APNs CSR, sign at Apple Push Certificates Portal, upload back.
   - Connect ABM for ADE (automated device enrollment) if available.
   - Re-enroll your Mac via the MDM enrollment profile.
2. **Windows MDM**
   - Settings → Integrations → MDM → Windows → toggle on.
   - Enroll the Windows VM via Settings → Accounts → Access work or school → use your Fleet URL.
3. **iOS/iPadOS**: enroll a personal iPad via OTA enrollment URL (Hosts → Add hosts → iOS).
4. **Android**: connect Android Enterprise, enroll a test Android device.
5. Push one configuration profile per platform:
   - macOS: `.mobileconfig` (set wallpaper, disable AirDrop)
   - Windows: CSP XML (disable Cortana)
   - iOS: restrict camera or push a Wi-Fi profile
6. **Lock/Unlock**: pick one test host, Lock from the UI, Unlock with the PIN. **Do NOT wipe your daily driver.**
7. **Done when:** 4 platforms enrolled, 1 profile each, lock/unlock tested.

---

## Phase 10 — Certificate authorities + automatic SCEP/ACME renewal
**Goal:** Issue device certificates from a CA and let Fleet keep them valid automatically (new in Fleet v4.86).

1. Settings → Integrations → Certificate authorities. Add a CA. Options include Fleet's built-in SCEP proxy, a custom SCEP server, DigiCert, or an ACME (hardware-attested) source. For a sandbox, a custom SCEP CA is the simplest starting point.
2. Build a certificate configuration profile (macOS `.mobileconfig`, or Windows CSP) that requests a cert from your CA, using Fleet variables in the payload:
   - `$FLEET_VAR_CUSTOM_SCEP_CHALLENGE_<CA_NAME>` for the one-time SCEP challenge password (replace `<CA_NAME>` with the CA you configured in step 1).
   - **Required for auto-renewal:** put `$FLEET_VAR_CERTIFICATE_RENEWAL_ID` in the **Organizational Unit (OU)** field of the certificate's Subject. This is the marker Fleet uses to track and renew the cert.
3. Deploy the profile to a test host via GitOps (`lib/profiles/`) or Controls → OS settings → Custom settings.
4. Confirm issuance: Host details → host vitals shows the issued certificate (Fleet retrieves SCEP/ACME certs via the MDM `CertificateList` command when the profile installs).
5. **Automatic renewal (v4.86):** Fleet now re-pushes the profile before the cert expires, *including certs not proxied through Fleet*. This covers Okta conditional access (SCEP), Okta Verify (SCEP with a static challenge), and hardware-attested ACME. No extra config beyond `$FLEET_VAR_CERTIFICATE_RENEWAL_ID` in the OU.
6. Optional: issue a short-lived cert to watch a full renewal cycle happen on its own.
7. **Done when:** a host has a CA-issued cert in host vitals, the profile uses `$FLEET_VAR_CERTIFICATE_RENEWAL_ID` in the OU, and you can explain how Fleet auto-renews before expiry.

   > New in [Fleet 4.86.0](https://fleetdm.com/releases/fleet-4-86-0) (May 29, 2026): automatic renewal extended to SCEP and ACME certificates that aren't proxied through Fleet, reusing the same renewal logic as Fleet-proxied SCEP certs. See [Built-in variables](https://fleetdm.com/guides/fleet-variables) for the full `$FLEET_VAR_*` list.

---

## Phase 11 — macOS setup experience + bootstrap package
**Goal:** Zero-touch onboarding — the story customers care most about.

1. Build a bootstrap package: a signed `.pkg` that installs something trivial (e.g., a launchd plist that writes `/var/log/sandbox-bootstrap.log`).
2. Upload via Controls → Setup experience → macOS → Bootstrap package. Per team.
3. Add to GitOps: set `bootstrap_package:` to the download URL (see `fleetdm.com/guides/manage-boostrap-package-with-gitops`).
4. Configure end-user auth: Settings → Integrations → IdP → SAML (Okta dev tenant or Google Workspace).
5. Add setup-experience software + a setup script.
6. Wipe a test Mac, re-enroll via ADE, watch the full flow.
7. **Done when:** fresh-enrolled Mac runs DEP → bootstrap → setup software → setup script → desktop.

---

## Phase 12 — Scripts and host actions
**Goal:** Scripts as a primary remediation/automation surface.

1. Save 3 scripts in `lib/scripts/`:
   - macOS zsh: `system_profiler SPHardwareDataType`
   - Windows PowerShell: `Get-ComputerInfo`
   - Linux bash: `uname -a; lsb_release -a`
2. Reference them under `scripts:` in team yml.
3. Hosts page → host → Actions → **Run script**. Confirm output captured.
4. Verify the policy-failure script from Phase 7 ran automatically.
5. **Done when:** 3 scripts run via UI, 1 runs from a policy.

---

## Phase 13 — Slack integration
**Goal:** Human-in-the-loop notifications.

1. Create an incoming webhook in your Slack workspace for `#fleet-sandbox`.
2. Settings → Integrations → Automations. Wire up 3 automations to Slack:
   - Policy failure (FileVault policy)
   - Failing host count threshold
   - Vuln automation: CVSS ≥ 7 on any software
3. Trigger each, confirm messages arrive.
4. **Bonus:** build a small Slack bot (Bolt JS) that listens for `/fleet hosts` and calls the Fleet API. Host on Render free tier.

---

## Phase 14 — Tines workflow
**Goal:** Fleet → SOAR orchestration. Classic CSA demo.

1. Sign up for Tines Community Edition (free).
2. Build a story that:
   - Receives a webhook from Fleet on policy failure
   - Parses host + policy name
   - Looks up host owner via `GET /api/v1/fleet/hosts/:id`
   - Posts to Slack tagging the owner
   - Creates a Jira/Linear ticket (or just logs to a Tines event)
3. Repoint the Phase 7 webhook from `webhook.site` to Tines.
4. Force a failure, watch the full chain.

---

## Phase 15 — SIEM (log destinations)
**Goal:** Every query/policy/script result can ship to a SIEM.

1. Pick a free-tier SIEM: Elastic Cloud trial, Splunk Free, or Grafana Cloud + Loki.
2. In Fleet (env vars on Render): set `FLEET_OSQUERY_RESULT_LOG_PLUGIN` and `FLEET_OSQUERY_STATUS_LOG_PLUGIN`. Easiest for a sandbox is `webhook` pointing at a Loki/Logstash HTTP input. Other options: firehose, kinesis, kafka, pubsub, filesystem.
3. Schedule a noisy query (`SELECT * FROM processes LIMIT 50;` every 5 min on Workstations).
4. Confirm results land in the SIEM and are searchable.
5. Build 1 dashboard panel (e.g., "top processes by host").

---

## Phase 16 — Vuln management + ticketing
**Goal:** Close the loop from discovery to ticket.

1. Software → Vulnerabilities. Confirm CVEs populated (needs Premium + NVD sync time).
2. Settings → Integrations → Ticket destinations. Connect Jira Cloud (free) or Zendesk.
3. Settings → Integrations → Automations → Vulnerability automations → create ticket above a CVSS threshold.
4. Force detection by installing an old Firefox ESR. Confirm a ticket is created with host list + CVE details.

---

## Phase 17 — Conditional access
**Goal:** Walk through the "Fleet as a trust signal" story.

1. Settings → Integrations → Conditional access. Connect Microsoft Entra ID (free tenant) or Okta.
2. Define a "compliant" rule: must pass policies X, Y, Z from Phase 7.
3. Configure Entra/Okta to require Fleet compliance for one test SaaS app.
4. Test: compliant host signs in (works). Force a policy failure, retry (blocked).

---

## Phase 18 — API + fleetctl
**Goal:** Fluency in the two non-UI surfaces customers use.

1. Make 5 API calls with curl + your token:
   - `GET /api/v1/fleet/hosts`
   - `GET /api/v1/fleet/queries`
   - `POST /api/v1/fleet/queries/run` (live query)
   - `POST /api/v1/fleet/scripts/run`
   - `GET /api/v1/fleet/software`
2. Set up [Postman](https://www.postman.com/downloads/) with the Fleet API:
   1. Open Fleet's official [Postman collection](https://www.postman.com/fleetdm/workspace/fleet/collection/18010889-c5604fe6-7f6c-44bf-a60c-46650d358dde) and **Fork** it into your workspace (you need a free Postman account).
   2. Create an environment named `Fleet sandbox`. After forking, open the collection **Variables** tab — Postman lists every placeholder the collection expects (for example your Fleet server URL and API token). Copy those keys into your environment and set **Current value** for each (Postman only substitutes variables that have a current value).
   3. Set your Fleet server URL to your Render URL from Phase 1 (no trailing slash), e.g. `https://csa-sandbox-fleet.onrender.com`.
   4. Set your API token to the API-only user's token from Phase 4. Use **Bearer Token** auth (the collection may set this automatically once the token variable is filled).
   5. If the collection asks for `api_version`, set it to `v1`.
   6. Select the `Fleet sandbox` environment in the environment picker (top right), then send the same five requests from step 1 using the matching folders in the collection (Hosts, Queries, Scripts, Software).
   7. Optional: use the collection's **Log in** request (`POST /api/v1/fleet/login`) to fetch a token instead of copying it from the UI — useful when testing as a non-API-only user. SSO and MFA users must use **My account → Get API token** instead.

   > **Tip:** Running the collection's **Log out** request invalidates your token. If you get `401 Unauthorized` on later requests, generate a new token from the UI or log in again. When running the full collection via Collection Runner, skip Log out (and password-reset) requests to avoid this.

3. **Done when:** the five curl calls succeed, and the same five requests succeed in Postman against your sandbox.

---