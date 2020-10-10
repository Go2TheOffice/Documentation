# Hardware Specs of Deployed Machines

Note: The best utility I've found to look at hardware information in cli is `lshw` 

## 1. pve-1
- CPU: AMD Ryzen 5 3600
  - Headless
  - 6 core (12 vcpu)
## 2. pve-edge

#LOOKINTO It would be cool to write a script that probes the hardware once in a while and updates this doc. Maybe proxmox has a built in call for finding hardware information, I can then just call that command, format it, and then write it to a file as a cron job