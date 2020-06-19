# Virtual Appliance

## Prerequisites \(vProtect image\)

* [RHV/oVirt/Oracle Linux VM](rhv-ovirt-olvm-virtual-appliance.md)
* [Citrix XenServer/XCP-ng](citrix-hypervisor-or-xcp-ng-virtual-appliance.md)
* [VMware/ESXi ](../protected-platforms/virtual-machines/vmware-vsphere.md)

2 vCPU core and 4GB RAM

**FTP access \(to download image\) requires credentials which are provided in the e-mail with license**

### Default login, and password

* Operating system / SSH: login: **root** password: **vPr0tect**
* WebUI: login: **admin** password: **vPr0tect**

## First steps after deployment

After downloading and importing image to environment set IP address:

* run `nmtui` 
* `Edit a connection`
* Select network interface, and edit its network settings.

Then go to [Initial configuration](../initial-configuration.md) to set you backup destination, and add virtualization hosts.

