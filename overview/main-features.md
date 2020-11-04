# Main Features

* VM-level, Container-level backup and recovery
* Full and Incremental backups \(with native CBT, CRT where available\) 
* VM data is exported in native \(hypervisor-specific\) format
* Backup only selected VM disk option
* Deployment protection \(PVs and deployment metadata\) for Kubernetes and OpenShift environments
* Support for a wide range of virtual platforms, containers and cloud such as:
  * VMware vSphere
  * Microsoft Hyper-V
  * Red Hat Virtualization 
  * oVirt 
  * Oracle Linux Virtualization Manager
  * Nutanix Acropolis Hypervisor \(AHV\)
  * HPE SimpliVity
  * Citrix Hypervisor \(XenServer\) 
  * XCP-ng
  * Proxmox VE
  * Oracle VM
  * OpenStack
  * libvirt hypervisors \(KVM, PowerKVM, KVM for IBM z, Xen\)
  * Kubernetes
  * Red Hat OpenShift 
  * AWS EC2 
* Pre/post snapshot remote command execution on VM to enable operations such as DB quiesce
* Pre/post backup destination access command execution to allow mount/unmount operations on external storage providers
* Prioritized backup policies 
* VM automatic policy assignment based on regular expressions and tags
* Snapshot Management \(Copy Data Management\) 
* Snapshot consistent technology \(quiesced snapshots\)
* File-level restore using mountable backups 
* Mounted backups - RAW disks shareable over iSCSI \(for direct block-access to your backup data\)
* Recovery plans for automated DR - on-demand restore of multiple VMs when needed, or on a scheduled basis for testing
* Generic application backup with your custom scripts with ready templates for commonly used databases
* OpenStack and KVM stand-alone with Ceph RBD storage support
* Multi-node support for better scalability or geographically dispersed environments
* Build-in vProtect DB backup
* Data encryption for file system backup destination
* LDAP authentication 
* Integration with different backup destinations such as: 
  * Any mounted file-system \(local or remote, especially GlusterFS for replication, CephFS, NFS, SMB and many more\) 
  * Dell EMC Data Domain \(BoostFS integration\)
  * Amazon S3 \(with Amazon Glacier as a 2nd tier archive storage\), 
  * S3-compliant storage \(IBM Cloud, Oracle Cloud, Scality RING\)
  * Google Cloud Storage
  * Microsoft Azure Blob Storage
  * OpenStack Swift 
  * IBM Spectrum Protect \(TSM\)
  * Dell EMC Networker
  * Dell EMC Avamar
  * Veritas Netbackup
* Built-in data deduplication with Virtual Data Optimizer \(VDO\)
* Easy to use and modern management \(HTML5 web UI and CLI\)
* Open API for 3rd party software integration \(REST API\)

