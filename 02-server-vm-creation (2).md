# Phase 2: Server 2022 VM Creation

## What I did
- Created new VM "DC02": Windows Server 2022 (64-bit), 4096MB RAM, 2 CPUs, 60GB dynamically allocated VDI
- Configured Network Adapter 1 as Host-Only Network (instead of default NAT)
- Attached Server 2022 ISO to optical drive, set boot order (Optical before Hard Disk)
- Ran through Windows Setup: Custom Install, selected Standard Evaluation (Desktop Experience)
- Set local Administrator password on first login

## Why I made these choices

**Host-Only instead of NAT:**
I used Host-Only instead of NAT due to the purpose of this project (Active Directory). This was used so I could communicate between the DC and the client VM. The reasoning for NAT not supporting this is that it would require the devices to be on isolated networks, allowing internet access but not allowing communication between both devices. These devices need to be on the same network so it can be verified through the Domain Controller, which Host-Only provides.

**Desktop Experience instead of Server Core:**
Since I am still in the early stages of setting up Active Directory, I decided to go with Desktop Experience. Desktop Experience provides the guidance I needed by giving me a graphical interface and menus. I did not use Server Core because it is done entirely through commands in PowerShell, and I wanted to see the concepts visually before working with command-line-only administration.

**Custom Install instead of Upgrade:**
Custom Install was used because Upgrade was not applicable in this project, due to there being no OS to upgrade from. Custom allows me to start from scratch on a blank, unformatted disk.

**4GB RAM / 2 CPU allocation:**
I used 4GB of RAM and 2 CPUs to allocate the resources needed for this project's workload, without over-allocating.

## Screenshots

**Windows Server 2022 edition confirmation:**
![Server 2022 edition](../images/DC02OS.png)

**Host-Only network adapter configuration:**
![Host-only adapter](../images/hostonly.png)

## Problems I ran into

**1. Disk creation confusion / "Failed to open" error**
- What happened: Attempted to attach a newly created VDI and received a "Failed to open the disk image file" error. Previous VDI files from earlier attempts were still present and selectable in the wizard, and one turned out to be corrupted (opened as unreadable text instead of a valid disk image).
- Root cause: VirtualBox's internal media registry stayed out of sync after manually deleting files in File Explorer instead of through VirtualBox.
- Fix: Removed the old/leftover VDI files, then created a fresh virtual disk and confirmed the filename and size were correct before proceeding with VM creation.

**2. "No bootable device" error on first boot**
- What happened: VM tried to boot with an empty optical drive.
- Root cause: ISO wasn't actually attached to the optical drive despite going through the wizard.
- Fix: Manually attached ISO via Settings → Storage.

**3. Accidental install restart**
- What happened: Pressed a key during the "press any key to boot from CD" prompt mid-install reboot, which restarted the entire install.
- Lesson: Don't touch keyboard/mouse during automatic install reboots unless a prompt specifically asks for input.

**4. Mouse not responsive on first-login password dialog**
- What happened: VM was not recognizing mouse input on first attempt.
- Fix: Switched to keyboard-only navigation (Tab/Enter) on that specific screen.

## Key takeaways
I learned that NAT was the incorrect network setting for this project. NAT isolates devices from each other on the network — each VM gets internet access individually but cannot communicate with other VMs. Host-Only allows communication between the client and the DC, since they are placed on the same network, allowing my project to function the way it was intended.
