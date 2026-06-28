# 3. Fleets & labels

Understand and use Fleet's two grouping primitives: fleets and labels.

> 🏗️  Keep your hard hat on, this content is under construction.

## Fleets, labels, and an API-only user
**Goal:** Group your hosts and create the API-only user you'll need for GitOps.

>Documentation: [Fleets](https://fleetdm.com/guides/fleets) | [Labels](https://fleetdm.com/guides/managing-labels-in-fleet)

1. Create 2 fleets via Settings → Fleets: `Workstations` and `Servers`.
2. Move Mac + Windows into Workstations, Linux into Servers.
3. Create labels via the user dropdown → Labels
   - **Dynamic** label `macOS Sonoma+`: `SELECT 1 FROM os_version WHERE platform='darwin' AND major >= 14;`
   - **Manual** label `Test devices`: add your hosts by hand.

   > Dynamic labels are defined by a single osquery `SELECT` query — membership updates automatically as hosts report in. You don't need to know osquery in depth here; you'll go deeper in [Host data & queries](5-host-data-and-queries.md).

4. Create an **API-only user** (Settings → Users → Add user → select "API-only user"). Set **Permissions** to `Global user`, **Role** to `GitOps` and **API access** to `All API endpoints`. Save the token — you'll need it for GitOps.
5. **Done when:** 2 teams populated, 1 dynamic + 1 manual label working, API token saved.
