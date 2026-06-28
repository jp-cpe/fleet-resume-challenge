# 12. Certificates

Manage device certificates using Fleet.

## Goal
A host issued a CA-backed certificate via a configuration profile, with Fleet automatically renewing it before expiry.

## Resources
- [Step-by-step: Certificates](../guides/certificates.md)
- [Built-in Fleet variables](https://fleetdm.com/guides/fleet-variables)
- [Fleet 4.86.0 release notes](https://fleetdm.com/releases/fleet-4-86-0)

## Tasks
- [ ] Add a certificate authority in Fleet
- [ ] Build and deploy a certificate configuration profile using Fleet variables (including the renewal ID in the OU)
- [ ] Confirm issuance in host vitals and understand how auto-renewal works

## Done when
- [ ] A host has a CA-issued cert in host vitals, the profile uses `$FLEET_VAR_CERTIFICATE_RENEWAL_ID` in the OU, and you can explain how Fleet auto-renews before expiry

> 🏗️  Keep your hard hat on, this content is under construction.
