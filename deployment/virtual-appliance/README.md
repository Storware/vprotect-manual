# Virtual Appliance

## Prerequisites \(vProtect image\)

* [RHV/oVirt/Oracle Linux VM](rhv-ovirt-olvm-virtual-appliance.md)
* [Citrix XenServer/XCP-ng](citrix-hypervisor-or-xcp-ng-virtual-appliance.md)
* [VMware/ESXi](vmware-virtual-appliance.md) 
* [Nutanix Acropolis Hypervisor \(AHV\)](nutanix-virtual-appliance.md)

2 vCPU core and 4GB RAM

**FTP access \(to download image\) requires credentials which are provided in the e-mail with license**

### Default login, and password

* Operating system / SSH: login: **root** password: **vPr0tect**
* WebUI: login: **admin** password: **vPr0tect**

## First steps after deployment

After downloading and importing the image to the environment set IP address:

* run `nmtui` 
* `Edit a connection`
* Select network interface, and edit its network settings.

Now you should be able to log in to the web console using the URL:
```text
https://VPROTECT_HOST:8181
```
where VPROTECT_HOST is the hostname or IP of your vProtect Server


Then go to [Initial configuration](../initial-configuration.md) to set your backup destination, and add virtualization hosts.

**Note:** Importing the image will install both the server and node. If You need only the vProtect node, then You have to stop and disable vprotect-server service.

```text
systemctl stop vprotect-server
systemctl disable vprotect-server
```

You can register nodes installed manually \(RPM/Ansible\) to the vProtect server installed from OVA.

