# Enroll hosts — step-by-step

> Step-by-step companion to [Module 2: Enroll hosts into Fleet](../modules/2-enroll-hosts.md). Methods change fast; if anything here is out of date, defer to the linked Fleet docs.

## Create lab VMs with UTM
**Goal:** One disposable VM per OS so you can enroll, break, and re-enroll without touching your daily driver.

> Documentation: [UTM documentation](https://docs.getutm.app/)

1. Install [UTM](https://mac.getutm.app/) on your Mac.
2. **macOS** — Create a macOS VM (Apple Silicon) in UTM. Name it `fleet-sandbox-macos`. Complete setup with a local admin account.

   > Download an [IPSW](https://mrmacintosh.com/apple-silicon-m1-full-macos-restore-ipsw-firmware-files-database/).

3. **Windows** — Create a Windows 11 ARM VM (Apple Silicon) in UTM. Name it `fleet-sandbox-windows`. Complete setup with a local admin account.

   > Use [CrystalFetch](https://apps.apple.com/us/app/crystalfetch-iso-downloader/id6454431289?mt=12) to obtain the latest Windows installer.

4. **Linux** — Create an Ubuntu VM (ARM images on Apple Silicon). Name it `fleet-sandbox-ubuntu`. Complete setup with a local admin account.

   > Download [Ubuntu Desktop](https://ubuntu.com/download/desktop) or [Ubuntu Server](https://ubuntu.com/download/server)

5. **Networking** — Use Shared Network (NAT) in UTM so each VM has outbound internet. Confirm each guest can reach your Fleet Render URL in a browser or with `curl`.
6. Take a **snapshot** (Right-click > **Clone**) of each VM right after OS setup, before enrolling Fleet. Name them `windows-backup`, `macOS-backup`, `ubuntu-backup`, etc..
7. **Done when:** 3 VMs boot, reach the internet, and can resolve your Fleet URL.

## Install fleetctl and enroll first hosts
**Goal:** At least one of each major OS reporting in.

> Documentation: [Fleetctl documentation](https://fleetdm.com/guides/fleetctl)

1. [Install fleetctl](https://fleetdm.com/download)
2. Review the available commands by running `fleetctl --help` in Terminal.
3. [Authenticate to your Fleet instance](https://fleetdm.com/guides/fleetctl#authentication).
4. Back in the Fleet UI: Hosts → **Add hosts** → view the fleetctl commands needed to generate Fleet's agent for each OS type.

   > Run `fleetctl package --help` in Terminal to view all the options for creating Fleet installer packages.

5. Generate installers with `fleetctl package` for macOS (`.pkg`), Windows (`.msi`), and Linux (`.deb`/`.rpm`).

   > You will need to install [Docker.app](https://docs.docker.com/desktop/setup/install/mac-install/) to create the Windows installer from a Mac.

   >  Generate the Linux agent without fleet desktop.

6. Enroll your VMs (macOS, Windows, and Linux) by installing each package in its guest OS.

   > Tip: Create a new directory on your computer and add it as a shared directory for each VM in the UTM settings.

   > Tip: If you have trouble accessing the shared drive on any of the VMs it may be faster to generate a new installer on the VM itself by installing fleetctl from fleetdm.com/download and running the appropriate command.

7. Optional: ChromeOS, iOS/iPadOS, and Android or any other spare bare metal test devices you have laying around.
8. **Done when:** 3+ hosts show online.
