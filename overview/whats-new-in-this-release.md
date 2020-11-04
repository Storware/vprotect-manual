# What's new in this release

* New: Proxmox VE - the new backup strategy for Proxmox VE \(includes incremental backups, FS freeze, and option to share backups over iSCSI\)
* New: RHV/oVirt - new backup strategy using CBT \(using new RHV/oVirt backup APIs, currently in Technical Preview\)
* New: Nutanix AHV – option to connect via Prism Central instead of individual Prism Elements
* New: Nutanix AHV – option to automatically assign VMs to the policies based on Nutanix categories \(vProtect tags\) when using Prism Central as hypervisor manager
* New: Option to automatically power on VM after restore
* New: Option to automatically generate VM name with prefix/suffix in Recovery Plans
* New: Major Web UI update and reorganization
  * better arrangement of views related to the artifacts in the same domain, such as Virtual Environment-related schedules, policies etc.
  * Policies and Schedules are now in Virtual Environments menu - grouped as tabbed views in separate sections: "Backup SLAs", Snapshot SLAs and Recovery Plans
  * Hypervisor tab moved also to Virtual Environments -&gt; Infrastructure submenu
  * Mounted backups also now part of Virtual Environments menu
  * Audit log and dashboard backup reporting moved to separate Menu item
  * Nodes and Node Configurations tabs moved to separate submenus 
* New: Calendar-based view of backup/restore history with filtering in VM details
* New: S3/S3-compatible backup provider – Nutanix Objects support
* New: S3/S3-compatible backup provider – proxy support
* New: Kubernetes/OpenShift – token-based authentication
* New: Task APIs optimizations
* New: Support for Chrome SameSite cookie security flag
* New: Support for Red Hat OpenShift 4.5
* New: OpenStack - support for credentials using alternative domains
* New: oVirt/RHV UI plugin enhancements \(new VM details view, mounted-backup support, and policy/schedule management\)

