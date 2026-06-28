# Setup experience — step-by-step

> Step-by-step companion to [Module 13: Setup experience](../modules/13-setup-experience.md). Methods change fast; if anything here is out of date, defer to the linked Fleet docs.

## macOS setup experience + bootstrap package
**Goal:** Zero-touch onboarding — the story customers care most about.

1. Build a bootstrap package: a signed `.pkg` that installs something trivial (e.g., a launchd plist that writes `/var/log/sandbox-bootstrap.log`).
2. Upload via Controls → Setup experience → macOS → Bootstrap package. Per team.
3. Add to GitOps: set `bootstrap_package:` to the download URL (see `fleetdm.com/guides/manage-boostrap-package-with-gitops`).
4. Configure end-user auth: Settings → Integrations → IdP → SAML (Okta dev tenant or Google Workspace).
5. Add setup-experience software + a setup script.
6. Wipe a test Mac, re-enroll via ADE, watch the full flow.
7. **Done when:** fresh-enrolled Mac runs DEP → bootstrap → setup software → setup script → desktop.
