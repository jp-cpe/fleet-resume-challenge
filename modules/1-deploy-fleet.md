# 1. Deploy Fleet

You need to deploy Fleet.

## Phase 1 – Deploy Fleet

**Goal:** A working Fleet instance you own end-to-end.
> Documentation: [Deploy Fleet](https://fleetdm.com/docs/deploy/deploy-fleet)

1. Follow the above documentation to deploy Fleet.

    > Render is the simplest method, but costs $65/mo. You can also [deploy Fleet with Docker](https://fleetdm.com/guides/deploy-fleet-on-docker-compose) for free.

2. You can grab a 3-day Fleet premium license key on the [Fleet website](https://fleetdm.com/login#).

    > Add `FLEET_LICENSE_KEY` as a web service enironment variable. Without it, you can't test any premium features. [Learn more](https://fleetdm.com/docs/configuration/fleet-server-configuration#license-key).

4. Open the public URL of your Fleet server, complete setup (name/email/password).

5. **Done when:** UI loads, Hosts page is empty, and My account page shows a license expiration date.
