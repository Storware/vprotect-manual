# RHV/oVirt/OLVM Virtual Appliance

## Installation

1. Upload OVA image to RHV host storage, or repository.
2. Login to RHV manager.
3. Go to tab Compute -&gt; Virtual Machines.
4. On right bottom of screen click on tree dots, and chose "Import".
5. Select source "Virtual Appliance \(OVA\).
6. Select host with uploaded OVA image.
7. Enter "File Path" to OVA image file.
8. Click button "Load".
9.  Move from list "Virtual MAchines on Source" our image "DELL EMC vProtect Virtual Appliance" to list "Virtual Machines to Import".
10. Click "Next".
11. Chose your target Storage Domain, Cluster, CPU Profile, Allocation Policy.
12. In tab "General" edit Name.
13. In tab "Network Interfaces" chose Network for your nic's in VM.
14. Click "OK", import task is started.
15. When VM is imported and running login to SSH to get VM IP, and next you can access vProtect by WebUI.

## Deinstallation

1. Remove VM from virtual enviroinment.
2. If backup destination is outside VM, then remove backup's files from backup destination.

## Credentials

### SSH

user name: root

password: vPr0tect

### WebUI

URL: [https://IP](https://IP)

user name: admin

password: vPr0tect

### MariaDB

user name: root

password: vPr0tect

