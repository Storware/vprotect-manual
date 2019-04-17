# Planning

Before you proceed with installation please take a few minutes to read key concepts and requirements to properly plan installation.

vProtect consists of 2 main components:

* **vProtect Server** - central point of vProtect management, provides administrative Web UI and APIs and  is a central repository of metadata
* **vProtect Node** - data mover - you can have multiple nodes doing actual backups/restores etc. - all of them are managed by the Server and need to be registered to the server

## Placement

* vProtect Server and Node can be installed together
* Server can be installed anywhere \(physical box or VM\) - nodes just need to be able to connect to it
* Nodes can be installed also inside a VM or physical box, but keep in mind that some backup strategies require Node to be installed as a VM on a Hypervisor Cluster \(especially when "disk attachment" export mode is mentioned\)
* Both components assume they are installed on a CentOS 7 minimal
* 90% of installation cases is a single VM with both installed together residing on the virtualization platform that you want to backup

## Network

* Critical for data transfer, so first you need to understand which paths are used for backups as in many cases you're going to use LAN
* Internet is in general not required, but during the installation `yum` needs to fetch packages from the repositories, so you need at least access to your internal repositories
* **Storware Insight** \(enhancement allowing Storware to know about your problems earlier\) needs Internet access from the Server
* Node needs access to the Server \(ports 443 or 8181 depending on the setup\)
* Node needs connectivity with backup providers \(if they are external, such as S3, IBM Spectrum Protect etc.\)
* Nodes needs connectivity with the Hypervisor or Hypervisor Manager

## Installation options

For some platforms \(Citrix XenServer, oVirt/RHV, VMware\) you may use ready Virtual Appliances downloadable from FTP. You should check corresponding sections here: [vProtect Virtual Machine deployment](image/).

Other platforms require preparation of CentOS 7 VM or physical installation and following regular [Installation](install/) steps.

## High level installation steps

1. Deploy VM image as described in [vProtect Virtual Machine deployment](image/) **or** follow [Installation](install/) steps
   * In any case - Node requires **staging space** in `/vprotect_data` - assume number of concurrent export and store tasks and multiply it by biggest VM size \(**for example:** 6 export tasks + 4 store tasks \* 100 GB should require around 1 TB\)
   * Virtual Appliance may not have the most recent version of the packages - we recommend to download RPMs from the FTP anyway and [Update](update.md) if necessary
2. Now it is time for [Initial configuration](initial_config/)
3. Add to your Hypervisor\(s\) or Hypervisor Manager\(s\) and initiate index to retrieve list of VMs, storage etc.:
   * By indexing Hypervisor Manager \(oVirt/RHV/Oracle VM/Nutanix Prism\) you'll have hypervisors added automatically
   * Only stand-alone \(not managed by any sort of manager\) hypervisors are needed to be entered one by one \(KVM/Xen/Citrix XenServer/Proxmox VE\)
   * check appropriate sub-section for your platform: [Virtualization platforms](initial_config/virtualization-platforms/)
4. Setup backup provider:
   * check appropriate sub-section for your provider: [Backup providers](initial_config/backup-providers/)
5. Initiate test backup to verify that integration is completed.



