# Proxmox guide
This guide is a collection of notes used when working with Proxmox. As stated in the [[README]], this is not exhaustive, but is a collection of all of the gotchas and specific steps I took to get it working.

## Installation
Notes on what I had to deal with, troubleshooting and learnings from installing proxmox like 7 times

## Debian install 
I had a lot of trouble using the proxmox installer. After hitting so many issues, I switched to installing debian, then installing proxmox on top of it. Many of the forum users tend to prefer that anyway, and gives you tighter control during the setup.

### Install parameters
- Most details are [here](https://pve.proxmox.com/wiki/Install_Proxmox_VE_on_Debian_Buster)
- Install everything on one partition with LVM. When I separated out /home it caused storage confituration issues (not hard to solve) down the line
- Only install basic system utils, no GUI, no SSH etc. Proxmox provides it's own version of everything
- Follow the few steps that the guide tells you to post install

### Troubleshooting
#### Hard drive doesn't appear in the install menu
This issue happened on a machine with an [Intel Optane](https://store.hp.com/us/en/tech-takes/what-is-intel-optane-memory) chip. I think you might be able to switch back to using it once the OS is installed, but for the installation, debian really doesn't like it. [reference](https://askubuntu.com/questions/99038/why-does-the-ubuntu-installer-not-detect-the-hard-drive-during-installation)

#### vmlinuz-x.x.x-pve has invalid signature/load kernel first
You didn't disable secure boot dummy. Go to the bios and disable that. [reference](https://forum.proxmox.com/threads/vmlinuz-5-0-21-3-pve-has-invalid-signature.59479/)

**How to fix**
- Boot into bios menu
- Find storage/SATA settings
- Change SATA type to ACHI

## Maintenance/post install notes
After install, there may still be some stuff that needs to be fleshed out.

### Proxmox repositories aren't signed, apt cannot update
Remove the Proxmox enterprise repository (`pve-enterprise`) from `/etc/apt/sources.list` or from the `/etc/apt/sources.list.d/` directory. You can't access the enterprise repo unless you pay for Proxmox. May also need to add the community repositories instead. [reference](https://it42.cc/2019/10/14/fix-proxmox-repository-is-not-signed/)

### How do I make API calls?
Proxmox has a JSON API. See more [[Proxmox API|here]]

### Cannot access proxmox or other services through OpenVPN, even though other services are available
#TODO Document this stuff:
https://docs.netgate.com/pfsense/en/latest/virtualization/virtualizing-pfsense-with-proxmox.html#configuring-pfsense-software-to-work-with-proxmox-virtio

https://docs.netgate.com/pfsense/en/latest/virtualization/virtio-driver-support.html

https://pve.proxmox.com/wiki/PfSense_Guest_Notes

https://bugs.freebsd.org/bugzilla/show_bug.cgi?id=165059

