# 7. Software

Deliver software to your hosts.

> **Requires Fleet Premium** — free to complete with a Fleet Premium trial (see [Module 1](1-deploy-fleet.md)).

## Goal
Software delivered through every Fleet path: install-on-enroll, self-service, Fleet-maintained apps, and (optionally) VPP, with inventory and vulnerability data visible.

## Resources
- [Step-by-step: Software delivery](../guides/software.md)
- [Fleet-maintained apps](https://fleetdm.com/guides/fleet-maintained-apps)
- [How to use policies for patch management in Fleet](https://fleetdm.com/guides/how-to-use-policies-for-patch-management-in-fleet)
- [GitOps reference documentation](https://fleetdm.com/docs/configuration/yaml-files#fleet-maintained-apps)

## Tasks
- [ ] Deploy packages via GitOps, including an install-on-enroll package and a self-service package
- [ ] Add a Fleet-maintained app and (optionally) a VPP / App Store app
- [ ] Trigger an uninstall and confirm software inventory + CVEs appear

## Done when
- [ ] 2+ packages deployed, 1 self-service install completed from an end-user device, vuln data visible

