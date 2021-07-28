# RHV/oVirt/OLVM Virtual Appliance

## Files in this package:
- vprotect-va-rhv-XXX.ova - OVA image of virtual machine to deploy in RHV enviroinment.
- SHA256SUM - SHA-256 checksum file for other files.

## Installation
1. Unpack tgz archive
```text
tar -xvf vprotect-va-rhv-XXX.tgz
```
2. Upload OVA image to RHV host storage, or repository.
3. Login to RHV manager.
4. Go to tab Compute -> Virtual Machines.
5. On right bottom of screen click on tree dots, and chose "Import".
6. Select source "Virtual Appliance (OVA).
7. Select host with uploaded OVA image.
8. Enter "File Path" to OVA image file.
9. Click button "Load".
10. Move from list "Virtual MAchines on Source" our image "DELL EMC vProtect Virtual Appliance" to list "Virtual Machines to Import".
11. Click "Next".
12. Chose your target Storage Domain, Cluster, CPU Profile, Allocation Policy.
13. In tab "General" edit Name.
14. In tab "Network Interfaces" chose Network for your nic's in VM.
15. Click "OK", import task is started.
16. When VM is imported and running login to SSH to get VM IP, and next you can access vProtect by WebUI.
    
## Deinstallation
1. Remove VM from virtual enviroinment.
2. If backup destination is outside VM, then remove backup's files from backup destination.

## Credentials
### SSH

user name:  root

password:   vPr0tect

### WebUI
URL:    https://IP

user name:  admin

password:   vPr0tect

### MariaDB
user name:  root

password:   vPr0tect
