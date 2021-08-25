# What's new in this release

## vProtect 4.2 -&gt; 4.3

**Changes:**

* New: Huawei FusionCompute technical preview support
* New: Storage Provider - Nutanix Volume Groups support
  * volumes shared by multiple VMs can be backed up using this new storage provider
* New: Synthetic backup with DD Boost FS
  * forever-incremental capability using DD Boost FS - allows you also to restore backups without merging all incremental backups
* New: S3 backup provider - support for Cloudian S3
* New: S3 backup provider - support for Alibaba Cloud OSS
* New: RBAC - VM/Application/Storage instance-level permissions
  * option to restrict VMs/Storage visible in UI for specific users to the specific VMs \(limited to cluster, hypervisor, project, etc.\) and allow only specific actions on them
* New: Backup quotas for Openstack projects
  * option to configure limits for number of backups \(total backup size\) per Project in OpenStack environments
* New: Notification rules
  * this option allows you to create more complex notification rules, in which recipients should be informed about node unavailability, task failures etc.
* New: VMware - support for node installation on CentOS 8/Stream
* New: option to disable scheduled backups per policy
  * switch to enable/disable all scheduled backups per policy
* New: Web UI - backup/restore report export in PDF and HTML formats
  * option to export backup/restore reports in PDF or HTML format
* New: Web UI - configuration wizard with added support for Storage Providers and additional policy types
  * new configuration wizard sections to support Storage Providers
* New: Web UI - task duration in console
  * live timer showing for how long a task has been running so far
* New: Retention period extending to three years
  * maximum retention configurable in backup destination has been extended to three years