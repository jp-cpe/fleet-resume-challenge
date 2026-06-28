# 2. Enroll hosts into Fleet

Enroll hosts of at least 3 different operating systems into your Fleet server. 

> Any spare test devices or virtual machines will do. Fleet supports Windows, macOS, iOS, iPadOS, Android, and Linux.

## Goal

At least three hosts enrolled and reporting in to your Fleet server.

## Resources

Pick the virtualization tooling that matches the computer you're working from:

**If your computer runs macOS** (use UTM):

- [Step-by-step: Enroll hosts with UTM VMs and fleetctl on macOS](../guides/enroll-hosts.md)
- [UTM documentation](https://docs.getutm.app/)

**If your computer runs Windows** (use Hyper-V):

- [Enable Hyper-V on Windows](https://learn.microsoft.com/en-us/windows-server/virtualization/hyper-v/get-started/install-hyper-v?tabs=powershell&pivots=windows#enable-hyper-v)
- [Create a virtual machine in Hyper-V on Windows](https://learn.microsoft.com/en-us/windows-server/virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v)

**For any host computer:**

- [fleetctl documentation](https://fleetdm.com/guides/fleetctl)
- [Fleet agent / fleetctl download](https://fleetdm.com/download)

## Tasks

- [ ] Stand up at least two disposable test hosts of different operating systems that you can enroll and re-enroll
- [ ] Generate Fleet's agent installers for each platform
- [ ] Enroll each host and confirm it reports in
- [ ] (Optional) Enroll additional platforms: iOS/iPadOS, Android, or spare bare metal

## Done when

- [ ] 2+ hosts across macOS, Windows, and Linux show online in Fleet