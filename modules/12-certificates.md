# 12. Certificates

Manage device certificates using Fleet.

> 🏗️  Keep your hard hat on, this content is under construction. 

## Phase 1 — Certificate authorities + automatic SCEP/ACME renewal
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