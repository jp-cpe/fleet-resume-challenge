# Deploy Fleet — step-by-step

> Step-by-step companion to [Module 1: Deploy Fleet](../modules/1-deploy-fleet.md). Methods change fast; if anything here is out of date, defer to the linked Fleet docs.

## Deploy Fleet
**Goal:** A working Fleet instance you own end-to-end.

> Documentation: [Deploy Fleet](https://fleetdm.com/docs/deploy/deploy-fleet)

1. Follow the above documentation to deploy Fleet.

    > [Deploying Fleet with Docker Compose](https://fleetdm.com/guides/deploy-fleet-on-docker-compose) (or locally) is a $0 hosting path. Render is the simplest hosted option but is an optional convenience at ~$65/mo.

2. Grab a **free Fleet Premium trial license key** at [Try Fleet](https://fleetdm.com/try-fleet) — log in or create an account; it's free self-service, no sales call required. The trial license is valid for 30 days.

    > Add `FLEET_LICENSE_KEY` as a web service environment variable. Without it, you can't test any Premium features. [Learn more](https://fleetdm.com/docs/configuration/fleet-server-configuration#license-key).

    > You need the Premium trial to complete the Premium-flagged modules: Software (7), disk encryption (10), OS updates (11), certificates (12), setup experience (13), vulnerability management (14), conditional access (Bonus 3), plus creating fleets in Module 3. Plan to work through these within the 30-day trial window.

3. Open the public URL of your Fleet server, complete setup (name/email/password).
4. **Done when:** UI loads, Hosts page is empty, and My account page shows a license expiration date.
