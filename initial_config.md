# Initial configuration

## Server

1. Upload your license key:
	
	* log in to the web UI and go to the `Settings -> License` and upload your `license.key` file

1. It is **highly recommended** to setup periodic **DB backup** on vProtect Server. Setup a cron job to do a periodic backup and transfer it to secure location to protect your configuration from being lost. Please find below commands required to backup and restore vProtect Server database. **Note**: there is no space after `-p`.

  * DB backup:

	  ```
mysqldump -u root -pDBPASSWORD vprotect | gzip -9 > PATH_TO_GZIPPED_BACKUP
	  ```
  * In case of any issues - you can later restore DB with:

	  ```
gunzip < PATH_TO_GZIPPED_BACKUP | mysql -u root -pDBPASSWORD vprotect
	  ```

1. Admin account setup:

   * for audit purposes it is recommended to add individual admin accounts using `Users` section (accessible through `Users` menu item)
   * **Notice** - make sure to set the correct **time zone** for each user - default admin account has **UTC** by default.


## Server/Node

1. Setup backup destinations:
  * install any required binaries (IBM Spectrum Protect/Dell-EMC NetWorker/Veritas NetBackup on your **node**
  * add backup destination from UI or CLI and provide necessary parmeters as described in [Backup destinations](admin_webui_bd.md) section.

1. Add your HV managers (RHV/oVirt/Oracle VM) or Hypervisors (KVM/Xen/Citrix XenServer/Proxmox):
  * check for any specific requirements in the following sections (some require additional steps to be run on **node**):
     * [Citrix XenServer Change Block Tracking setup](setup_citrix_cbt.md)
     * [RHV/oVirt setup](setup_rhv.md)
     * [Nutanix setup](setup_nutanix.md)
     * [Oracle VM setup](setup_ovm.md)
     * [Proxmox setup](setup_proxmox.md)
     * [KVM (stand-alone)/Xen (legacy) setup](setup_kvm_xen.md)
  * add Hypervisors or HV managers using UI or CLI

1. Optionally, if you have multiple node setup - to enable centralized logs view from all nodes you need to export as NFS shares `/opt/vprotect/logs/<NODE_NAME>` directories and mount them on nodes in the same location.

  Example:
  vProtect01.lab.local 10.50.1.27 installed vprotect-server, and vprotect-node
  vProtect02.lab.local 10.50.1.28 installed vprotect-node
  Node vProtect02.lab.local collecting logs in folder `/opt/vprotect/logs/vProtect02.lab.local`
  Log in to vProtect02, and create NFS share for that localization, edit file `/etc/exports`

  `/opt/vprotect/logs/vProtect02.lab.local 10.50.1.27(rw,sync,insecure,all_squash,anonuid=993,anongid=990)`

  reload NFS configuration
  `exportfs -ra`

  Next login to vProtect01 and mount exported log folder.
  `mkdir /opt/vprotect/logs/vProtect02.lab.local`
  `chown vprotect:vprotect /opt/vprotect/logs/vProtect02.lab.local -R`

  For automatically mount logs, add new line to `/etc/fstab`:
  `10.50.1.28:/opt/vprotect/logs/vProtect02.lab.local /opt/vprotect/logs/vProtect02.lab.local nfs defaults  0 0`

1. Optionally, for RHV with disk attachment mode being used and Nutanix - follow these steps: [LVM setup on vProtect Node for disk attachment backup mode](setup_lvm.md)