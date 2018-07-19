# Hypervisor details and settings

Create/edit form for every `Hypervisor` enables you to specify hypervisor credentials and optionally change node assignment and import/export mode.


## Settings

You can specify the following settings for a hypervisor:

* `Host`, `Username`, `Password` - credentials to access your hypervisor 
* `Node` - specifies node with is supposed to handle backup process for VMs on this hypervisor   
   * if hypervisor has been added as a part of HV manager index task, it gets node of the manager by default - you can override this here if necessery
* `Import/export mode` - (Citrix only) defines the way the backups are done. Citrix XenServer supports 2 modes:
  * Full VM image (in XVA format, optionally compressed) with separate disk files for incremental backups (in VHD format):
     * XenServer 6.5
     * no file-level restore
     * no selective disk backup
  * Separate disks (full and incremental backups) with Changed Block Tracking (for incremental), which exports disks and metadata as separate files (RAW format):
     * supports selective disk backup
     * incremental backups are done in CBT mode - XenServer 7.3+ with CBT feature required
     * supports file-level restore (mounted backups)