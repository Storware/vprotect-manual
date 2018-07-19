# Virtual Machine details and settings

Details page for every `Virtual Machine` consists of several tabs:

* `Backups` - list of all successful backups
* `Backup History` - list of all backups that have been done (including backup jobs that failed)
   * note that you can download node logs for the given backup to check what was the reason of failure
   * backup details should also contain file system information (if file-level restore is supported for your platform) and information about list of files that have been backed up
* `Snapshots` - (Citrix only) - list of snapshots that are present on the hypervisor (left for incremental backup)
   * note that there is a green dot icon to indicate which snapshot is the one since which CBT mechanism will export data next time
* `Disks` - (Citrix only) - list of disks that have been detected for the VM
   * note that option to exlcude selected disks is only available if VM has effectively export/import mode set to `Separate disks (full/incremental)...`
* `Schedules` - list of all schedules assigned to the group to which VM belongs

**Note:** removal of any VM removes data from the backup provider - you should see 1 remove task per each backup destination that holds backups of the VM being removed

## Settings

You can specify the following settings for a VM:

* `Quiesce/freeze before snapshot` - (Citrix/RHV/oVirt only) this enables quiesced snapshots on Citrix XenServer and freezes filesystems before snapshot on RHV/oVirt VMs
* `Import/export mode` - (Citrix only) defines the way the backups are done. Citrix XenServer supports 2 modes:
  * Full VM image (in XVA format, optionally compressed) with separate disk files for incremental backups (in VHD format):
     * XenServer 6.5
     * no file-level restore
     * no selective disk backup
  * Separate disks (full and incremental backups) with Changed Block Tracking (for incremental), which exports disks and metadata as separate files (RAW format):
     * supports selective disk backup
     * incremental backups are done in CBT mode - XenServer 7.3+ with CBT feature required
     * supports file-level restore (mounted backups)
  * Inherited from HV - one of the above-mentioned settings, defined on the hypervisor level