# Getting started

Before you proceed with installation please take a few minutes to read key concepts and requirements to properly plan installation.

In this guide we assume that you're familiar with basic Linux administration \(especially shell\), your virtualization platform and backup provider that you want to use.

## Architecture

High level picture: vProtect gets data from your virtualization platform and stores it backup provider of your choice. Notice, that one of the backup providers supported is just file-system \(which can be almost anything including these that support deduplication such as VDO\), which allows to use vProtect as a stand-alone solutions without third-party backup providers.



![](../.gitbook/assets/general.png)

To be more precise - vProtect consists of 2 main components:

* **vProtect Server** - central point of vProtect management, provides administrative Web UI and APIs and  is a central repository of metadata
* **vProtect Node** - data mover - it does the dirty job: backups/restores/mounts
  * you can have multiple nodes doing actual backups/restores etc.
  * all of them are managed by the Server and need to be registered to the server

## Component placement

* **vProtect Server and Node can be installed together** - this is the most common case and 4 GB of RAM + 2 vCPU should be enough to run both.
* Server can be installed anywhere \(physical box or VM\) - nodes just need to be able to connect to it.
* Nodes can be installed also inside a VM or physical box, but keep in mind that some backup strategies require Node to be installed as a VM on a Hypervisor Cluster \(especially when "disk attachment" export mode is mentioned\).
* Both components assume they are installed on a CentOS/RHEL 7 minimal
* VDO \(file system that enables deduplication\) is available in both CentOS and RHEL, but we recommend to install Red Hat Enterprise Linux to have Red Hat support available in case of any issues with VDO.

For detailed deployment scenarios please refer to the following sections:

* [Deployment in VMware vSphere/ESXi environment](deployment-in-vmware-vsphere-esxi-environment-beta.md)
* [Deployment in Hyper-V environment](deploymnt-in-hyper-v-environment.md)
* [Deployment in oVirt/RHV environment](deployment-in-ovirt-rhv-environment.md)
* [Deployment in Nutanix AHV environment](deployment-in-nutanix-ahv-environment.md)
* [Deployment in Oracle VM environment](deployment-in-oracle-vm-environment.md)
* [Deployment in Proxmox VE environment](deployment-in-proxmox-ve-environment.md)
* [Deployment in Citrix XenServer environment](deployment-in-citrix-xenserver-environment.md)
* [Deployment in KVM/Xen environment](deployment-in-kvm-xen-environment.md)
* [Deployment in Kubernetes/OpenShift environment](deployment-in-kubernetes-openshift-environment.md)
* [Deployment in AWS EC2 environment](deployment-in-aws-ec2-environment.md)

## Network considerations

* Critical for data transfer, so first you need to understand which paths are used for backups as in many cases you're going to use LAN. Depending on where node is located you need to verify if data is not going to pass via low-bandwidth links.
* Internet is in general not required, but during the installation `yum` needs to fetch packages from the repositories, so you need at least access to your internal repositories.
* **Storware Insight** \(enhancement allowing Storware to know about your problems earlier\) needs Internet access from the Server.
* Node needs access to the Server \(ports 443 or 8181 depending on the setup\).
* Node needs connectivity with backup providers \(if they are external, such as S3, IBM Spectrum Protect etc.\).
* Nodes needs connectivity with the Hypervisor or Hypervisor Manager.

## Installation options

For some platforms \(Citrix XenServer, oVirt/RHV, VMware\) you may use ready pre-configured Virtual Appliance downloadable from FTP. You should check corresponding sections here: [vProtect Virtual Machine deployment](../image/).

Other platforms require preparation of CentOS 7 VM or physical installation and following regular [Installation](../install/) steps.

## OK, let's do it!

1. Choose your installation path. You have the following options:
   * [All-in-one quick installation](../install/all-in-one-quick-installation.md)
   * [Installation using Ansible playbook](../install/installation-using-ansible.md)
   * [Installation with RPMs](../install/installation-using-rpms.md)
   * [vProtect Virtual Appliance deployment](../image/)
2. Regardless of the installation option you choose:
   * Node requires **staging space** - assume number of concurrent export and store tasks and multiply it by biggest VM size \(**for example:** 6 export tasks + 4 store tasks \* 100 GB should require around 1 TB\)
   * We recommend it to be on a separate drive:
     * if you're going to test deduplicated File system backup destination - leave it not initialized
     * otherwise [Staging space configuration](../install/staging-space-configuration.md) will guide you to prepare storage on the spare drive
   * Virtual Appliance may not have the most recent version of the packages - we recommend to use repo later anyway and [Update](../update.md) if necessary
3. Now it is time for [Initial configuration](../initial_config/)
4. Add to your Hypervisor\(s\) or Hypervisor Manager\(s\) and initiate index to retrieve list of VMs, storage etc.:
   * by indexing Hypervisor Manager \(oVirt/RHV/Oracle VM/Nutanix Prism\) you'll have hypervisors added automatically. Other hypervisors are needed to be entered one by one \(KVM/Xen/Citrix XenServer/Proxmox VE\)
   * check appropriate sub-section for your platform: [Virtualization platforms](../initial_config/virtualization-platforms/)
5. Setup backup provider:
   * check appropriate sub-section for your provider: [Backup providers](../initial_config/backup-providers/)
   * we also recommend to check [Backup destinations](../admin_webui_overview/admin_webui_bd.md) section for detailed description of properties
6. Test basic operations to verify that integration is completed:
   * [How to backup](../admin_webui_overview/admin_webui_how_to_backup.md)
   * [How to restore](../admin_webui_overview/admin_webui_how_to_restore.md)
   * [How to mount](../admin_webui_overview/admin_webui_how_to_mount.md)

We recommend also to watch this [video](https://www.youtube.com/watch?v=c3PnfXG5Fs4), which presents a complete vProtect setup with several virtualization platforms and backup providers.



