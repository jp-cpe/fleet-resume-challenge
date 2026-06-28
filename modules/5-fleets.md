# 5. Fleets

Understand and use fleets as a grouping primitive.

## Phase 1 — Fleets, labels, and an API-only user
**Goal:** Understand Fleet's two grouping primitives: Fleets and Labels.

>Documentation: [Fleets](https://fleetdm.com/guides/fleets) | [Labels](https://fleetdm.com/guides/managing-labels-in-fleet)

1. Create 2 fleets via Settings → Fleets: `Workstations` and `Servers`.
2. Move Mac + Windows into Workstations, Linux into Servers.
3. Create labels via the user dropdown → Labels
   - **Dynamic** label `macOS Sonoma+`: `SELECT 1 FROM os_version WHERE platform='darwin' AND major >= 14;`
   - **Manual** label `Test devices`: add your hosts by hand.
4. Create an **API-only user** (Settings → Users → Add user → select "API-only user"). Set **Permissions** to `Global user`, **Role** to `GitOps` and **API access** to `All API endpoints`. Save the token — you'll need it for GitOps.
5. **Done when:** 2 teams populated, 1 dynamic + 1 manual label working, API token saved.