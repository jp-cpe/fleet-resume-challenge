# 6. Policies

Configure pass/fail policies in Fleet with auto-remediation.

## Goal
Cross-platform compliance policies with automated remediation, plus a working library of scripts you can run on hosts.

## Resources
- [Step-by-step: Policies and scripts](../guides/policies.md)
- [What are Fleet policies?](https://fleetdm.com/securing/what-are-fleet-policies)
- [Understanding the intricacies of Fleet policies](https://fleetdm.com/guides/understanding-the-intricacies-of-fleet-policies)
- [Fleet's community-maintained collection of policies](https://fleetdm.com/policies)

## Tasks
- [ ] Add 3 cross-platform policies to your GitOps repo
- [ ] Configure auto-remediation on one policy with a remediation script
- [ ] Wire a policy webhook automation to `webhook.site` and trigger a failure to confirm remediation and webhook delivery

## Done when
- [ ] 3 cross-platform policies exist in GitOps and are enforced in Fleet
- [ ] At least one policy has working auto-remediation, and a webhook fires with the correct payload on policy failure

