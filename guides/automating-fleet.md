# Automating Fleet (API & fleetctl) — step-by-step

> Step-by-step companion to [Module 15: Automating Fleet (API & fleetctl)](../modules/15-automating-fleet.md). Methods change fast; if anything here is out of date, defer to the linked Fleet docs.

## fleetctl (CLI)
**Goal:** Comfortably drive Fleet from the command line.

You installed `fleetctl` back in [Enroll hosts](../modules/2-enroll-hosts.md) and used it for [GitOps](../modules/4-gitops.md). Now explore it as a general-purpose tool.

> Documentation: [fleetctl](https://fleetdm.com/guides/fleetctl)

1. Run `fleetctl --help` and skim the command groups.
2. Authenticate with `fleetctl login`, then run a live query from the CLI with `fleetctl query`.
3. Pull data without the UI: `fleetctl get hosts`, `fleetctl get queries`.
4. **Done when:** you can run a live query and retrieve host/query data entirely from the CLI.

## REST API
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
   3. Set your Fleet server URL to your Render URL from [Deploy Fleet](../modules/1-deploy-fleet.md) (no trailing slash), e.g. `https://csa-sandbox-fleet.onrender.com`.
   4. Set your API token to the API-only user's token from [Fleets & labels](../modules/3-fleets-and-labels.md). Use **Bearer Token** auth (the collection may set this automatically once the token variable is filled).
   5. If the collection asks for `api_version`, set it to `v1`.
   6. Select the `Fleet sandbox` environment in the environment picker (top right), then send the same five requests from step 1 using the matching folders in the collection (Hosts, Queries, Scripts, Software).
   7. Optional: use the collection's **Log in** request (`POST /api/v1/fleet/login`) to fetch a token instead of copying it from the UI — useful when testing as a non-API-only user. SSO and MFA users must use **My account → Get API token** instead.

   > **Tip:** Running the collection's **Log out** request invalidates your token. If you get `401 Unauthorized` on later requests, generate a new token from the UI or log in again. When running the full collection via Collection Runner, skip Log out (and password-reset) requests to avoid this.

3. **Done when:** the five curl calls succeed, and the same five requests succeed in Postman against your sandbox.
