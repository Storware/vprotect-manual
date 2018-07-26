# Introduction

![](.gitbook/assets/intro%20%281%29.png)

## Storware vProtect 3.6 Manual

This is the official documentation for Storware vProtect software. Here you'll find all the information needed to setup and configure backup for your virtual infrastructure.

### About vProtect

Storware vProtect provides a crash-consistent backup of VMs running in open virtual environments. With easy management interface, you are able to quickly set up protection and store backups in several different backup providers.

#### Functional details:

* VM-level backup and protection
* Hybrid Protection \(2nd Tier can be at enterprise backup server or in the cloud\)
* Multi-node support for better scalability or geographically dispersed environments
* Disaster Recovery based on IBM Spectrum Protect \(TSM\), Dell-EMC Networker, Veritas Netbackup, Amazon S3, Microsoft Azure, OpenStack Swift or any mounted file-system \(local or remote\)
* Open API for 3rd party software integration \(REST API\)
* Support for libvirt hypervisors \(KVM, PowerKVM, KVM for IBM z, Xen\), Citrix XenServer, RHV/oVirt and Oracle VM environments
* Incremental backups \(CBT\) for Citrix XenServer, oVirt/RHV and Nutanix AHV 5.5+ hypervisors
* Option to backup only selected VM disks \(Citrix, Nutanix, KVM/Xen\)
* Build in data deduplication with VDO
* Data encryption for Amazon S3/Microsoft Azure/file system backup providers
* Prioritized backup
* Last backup can be kept on vProtect Server for faster recovery
* File-level restore using mountable backups \(Citrix XenServer, RHV/oVirt Nutanix AHV, KVM/Xen and Oracle VM\)
* RHV API v4 support with proxy VM backup strategy \(which doesn't require export storage domain\)
* VM auto-grouping based on regular expressions and tags \(Citrix, RHV/oVirt\)
* Data is exported in native \(hypervisor-specific\) format
* Snapshot consistent technology \(and use of quiesced snapshots for Citrix XenServer, Nutanix AHV and FS freeze in oVirt/RHV enviornments\)
* Pre/post snapshot remote command execution on VM to enable operations such as DB quiesce
* Pre/post backup destination access command execution to allow mount/unmount operations on external storage providers
* LDAP authentication in management console
* Easy to use and intuitive management \(HTML5 web UI and CLI\) - protect your virtual infrastructure in 3 easy steps:
  1. Connect to your infrastructure and backup provider
  2. Schedule backups or backup on demand
  3. Restore/mount VM backup if needed \(or just use local copy of last backup\)
     * To local vProtect storage
     * To any remote location mountable on vProtect Server
     * Directly to the virtual environment

### Contact

WWW: [http://www.storware.eu](http://www.storware.eu)

Support: [https://support.storware.eu/servicedesk/customer/portal/7](https://support.storware.eu/servicedesk/customer/portal/7)

E-mail: [info@storware.eu](mailto:info@storware.eu)

