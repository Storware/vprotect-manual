# Installation Overview

1. Choose your installation option. There are the following options:
   * [All-in-one quick installation](quick-install-all-in-one.md)
   * [Installation using Ansible playbook](installation-using-ansible-playbook.md)
   * [Installation with RPMs](installation-with-rpms.md)
   * [Virtual Appliance](virtual-appliance/README.md)
2. Regardless of the installation option you choose:
   * The node requires **staging space** - assume a number of concurrent export and store tasks and multiply it by the biggest VM size \(**for example:** 6 export tasks + 4 store tasks \* 100 GB should require around 1 TB\)
   * The [Staging space configuration](common-tasks/staging-space-configuration.md) will guide you to prepare storage on the spare drive
   * vProtect is installed in the `/opt/vprotect` folder and staging space is assumed to be in `/vprotect_data` - these are the defaults and should not be changed.
3. Now it is time for the [Initial configuration](initial-configuration.md)
4. Add your Hypervisor\(s\) or Hypervisor Manager\(s\) and initiate the index to retrieve a list of VMs, storage etc.:
   * by indexing Hypervisor Manager \(RHV/oVirt/OLVM/Oracle VM/Nutanix Prism\) you'll have hypervisors added automatically. Other hypervisors need to be entered one by one \(KVM/Xen/Citrix Hypervisor/XCP-ng/Proxmox VE\)
   * check the appropriate sub-section for your platform: [Virtualization platforms](protected-platforms/virtual-machines/)
5. Set up the backup provider:
   * you should check the backup destinations section for a detailed description of properties: [backup destinations](backup-destinations/)
6. Test basic operations to verify that integration is completed:
   * [How to backup](../administration/virtual-environments/instances/backup-on-demand.md)
   * [How to restore](../administration/virtual-environments/instances/restore-on-demand.md)
   * [How to mount](../administration/virtual-environments/file-level-restore-mounted-backup-1.md)

