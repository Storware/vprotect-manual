# Nutanix Acropolis Hypervisor \(AHV\) Virtual Appliance

## Installation

### Using Prism Element

1. Unpack ova file.

```text
tar -xvf vprotect-oVirt.ova
```

2. Upload qcow2 files
- Pull down the gear icon and click Image Configuration.
- In the Image Configuration screen, click + Upload Image.
- In the Create Image screen, specify the necessary details.
- Select DISK in the IMAGE TYPE drop-down list.
- Click Save.

3. Create VM with following parameters:
- vCPU: 4
- Memory: 8
- Add new disks: from "Operation" dropdown list select "Clone from Image Service" and select previously uploaded disk image


## Deinstallation

1. Remove VM from virtual environment.
2. If backup destination is outside VM, then remove backup files from backup destination.

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