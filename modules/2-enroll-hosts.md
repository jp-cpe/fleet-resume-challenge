# 2. Enroll hosts into Fleet

Enroll hosts of at least 3 different operating systems into your Fleet server.

## Goal
At least three hosts — macOS, Windows, and Linux — enrolled and reporting in to your Fleet server.

## Resources
- [Step-by-step: Enroll hosts with UTM VMs and fleetctl](../guides/enroll-hosts.md)
- [UTM documentation](https://docs.getutm.app/)
- [fleetctl documentation](https://fleetdm.com/guides/fleetctl)
- [Fleet agent / fleetctl download](https://fleetdm.com/download)

## Tasks
- [ ] Stand up one disposable test host per OS (macOS, Windows, Linux) you can enroll and re-enroll
- [ ] Generate Fleet's agent installers for each platform
- [ ] Enroll each host and confirm it reports in
- [ ] (Optional) Enroll additional platforms: ChromeOS, iOS/iPadOS, Android, or spare bare metal

## Done when
- [ ] 3+ hosts across macOS, Windows, and Linux show online in Fleet
