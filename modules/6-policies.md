# 6. Policies

Configure pass/fail policies in Fleet with auto-remediation.

## Goal
Cross-platform compliance policies with automated remediation, plus a working library of scripts you can run on hosts.

## Resources
- [Step-by-step: Policies and scripts](../guides/policies.md)

## Tasks
- [ ] Add 3 cross-platform policies to your GitOps repo (FileVault on macOS, BitLocker on Windows, auto-updates on Linux)
- [ ] Configure auto-remediation on one policy with a remediation script in `lib/scripts/`
- [ ] Wire a policy webhook automation to `webhook.site` and trigger a failure to confirm remediation and webhook delivery

## Done when
- [ ] 3 cross-platform policies exist in GitOps and are enforced in Fleet
- [ ] At least one policy has working auto-remediation, and a webhook fires with the correct payload on policy failure

