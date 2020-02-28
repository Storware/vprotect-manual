# Initial configuration

## Server

1. Upload your license key:
   * if you don't have it you can request it [here](https://storware.eu/licenses/vprotect)
   * log in to the web UI and go to the `Settings -> License` and upload your `license.key` file
2. It is **highly recommended** to setup [vProtect DB backup](vprotect-db-backup.md) - database is key to restore your vProtect environment and later all of the backups that you need.
3. Admin account setup:
   * for audit purposes it is recommended to add individual admin accounts using `Users` section \(accessible through `Users` menu item\)
   * **Notice** - make sure to set the correct **time zone** for each user - default admin account has **UTC** by default.

## Server/Node

1. Setup backup destinations:
   * install any required binaries \(IBM Spectrum Protect/Dell-EMC NetWorker/Veritas NetBackup on your **node**
   * add backup destination from UI or CLI and provide necessary parmeters as described in [Backup destinations](../admin_webui_overview/admin_webui_bd.md) section.
2. Add your HV managers \(RHV/oVirt/Oracle VM\) or Hypervisors \(KVM/Xen/Citrix XenServer/Proxmox\):
   * check for any specific requirements in the following sections \(some require additional steps to be run on **node**\):
     * [Citrix XenServer Change Block Tracking setup](virtualization-platforms/setup_citrix_cbt.md)
     * [RHV/oVirt setup](virtualization-platforms/setup_rhv.md)
     * [Nutanix setup](virtualization-platforms/setup_nutanix.md)
     * [Kubernetes/OpenShift setup](virtualization-platforms/kubernetes-setup.md)
     * [Oracle VM setup](virtualization-platforms/setup_ovm.md)
     * [Proxmox setup](virtualization-platforms/setup_proxmox.md)
     * [KVM \(stand-alone\)/Xen \(legacy\) setup](virtualization-platforms/setup_kvm_xen.md)
   * add Hypervisors or HV managers using UI or CLI
3. Optionally, for backup strategies involving **disk attachment** mode - follow these steps: [LVM setup on vProtect Node for disk attachment backup mode](setup_lvm.md)

