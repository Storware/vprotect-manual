# Changelog

## vProtect 4.3 (Nova)

**28.07.2021**

vProtect 4.3 brings a Nutanix Volume Groups storage provider and a technical preview of Huawei FusionCompute support, which means that now you can protect 20 different virtualization platforms and storage providers! Woo-hoo!

The synthetic backup provider now can also use Data Domain as the backend to provide both forever incremental and highly effective deduplication. Additionally, the S3 backup provider now also supports Cloudian S3 and Alibaba Cloud OSS.

This release has RBAC has also been extended to support instance-level permissions in the roles, so that administrators can control access even more granularly.

We also included additional UI enhancements such as configuration wizard changes, task duration in the task console, option to export reports in HTML or PDF format, or option to disable all schedules at the policy level.

**Changes:**

* New: Huawei FusionCompute technical preview support
* New: Storage Provider - Nutanix Volume Groups support
* New: Synthetic backup with DD Boost FS
* New: S3 backup provider - support for Cloudian S3
* New: S3 backup provider - support for Alibaba Cloud OSS
* New: RBAC - VM/Application/Storage instance-level permissions
* New: Backup quotas for projects
* New: Notification rules
* New: VMware - support for node installation on CentOS 8/Stream
* New: option to disable scheduled backups per policy
* New: Web UI - backup/restore report export in PDF and HTML formats
* New: Web UI - configuration wizard with added support for Storage Providers and additional policy types
* New: Web UI - task duration in console


## vProtect 4.2 (Nova)

**22.04.2021**

A brand new vProtect release - v4.2 adds synthetic backup provider based on XFS which allows forever-incremental backups and significantly faster restores. 

This release also introduces RBAC system-level roles to give administrators more control over vProtect security. User groups and roles enable administrators to specify which actions and views are accessible to which users, either by using build-in roles or define their own.

Version 4.2 adds snapshot-management for Ceph RBD Storage Provider and SMB support for Nutanix Files. Snapshot management has also been enabled for OpenStack with disk-attachment strategy.

Management interface have also been enhanced with additional notifications related to VDO space occupancy, transfer rate charts, inventory synchronization statuses and for oVirt/RHV/OLVM - option to easily configure
metadata DB backups directly from hypervisor manager's section.

For oVirt/RHV/OLVM distributions we also added 2 transfer optimizations - BZIP2 compression when using SSH Transfer and direct transfers to/from hypervisors in Disk Image Transfer strategy - both should result in even faster backups.

**Changes:**

* New: Synthetic file-system backup provider using XFS
* New: RBAC - system-level roles and user groups
* New: Storage Providers - Ceph RBD - snapshot management
* New: Storage Providers - Nutanix Files - SMB shares support
* New: OpenStack - snapshot management for disk-attachment strategy
* New: RHV/oVirt/OLVM - metadata DB backup setup from Hypervisor Manager details pane
* New: RHV/oVirt/OLVM 4.3+ - Disk Image Transfer now supports direct transfers to/from hypervisors
* New: RHV/oVirt/OLVM - data compression during transfer with BZIP2 for SSH Transfer
* New: Connectivity test action for Backup Destinations
* New: Connectivity test action for Virtualization Platforms
* New: Hyper-V multithreaded import during restore
* New: RHV/oVirt UI plugin update - now reflecting vProtect Main menu and views and with additional reporting
* New: Web UI - transfer rate charts
* New: e-mail notifications - when VDO runs out of space
* New: inventory synchronization status for Virtualization Platforms
* New: inventory synchronization status for Storage Providers


## vProtect 4.1 (Nova)

**28.01.2021**

vProtect 4.1 introduces a new type of backup source - storage provider. Now you can protect Ceph RBD volumes, plain file systems and - Nutanix Files (AFS) as technical preview. You can execute full and incremental backups, recover individual files using mounted backups or share them over iSCSI.

This release introduces also and important enhancement for multi-AZ OpenStack environments with separate Ceph clusters in each AZ. Now you can assign individual storage providers to each AZ ("hypervisor cluster"). For OpenStack and OpenShift environments, vProtect also adds Projects to its inventory during synchronization.

When using Dell EMC PowerProtect DD (Data Domain) as a backup provider, you can now enable retention lock feature to protect your backups. This release adds also backup size report which may be essential for chargeback reporting. Starting from 4.1 release Java 11 will be used.

**Changes:**

* New: Storage Provider - Ceph RBD - support for backup/restore/mount of RBD volumes
* New: Storage Provider - File system - support for backup/restore/mount of plain file systems mounted on the nodes
* New: Storage Provider - Nutanix Files (AFS) - technical preview of support for backup/restore/mount of Nutanix Files
* New: multi-AZ OpenStack environments support
* New: OpenStack/OpenShift - project scanning
* New: Backup size reporting for chargeback
* New: PowerProtect DD (DataDomain) retention lock
* New: Hyper-V multi-threated disk export
* New: Java update to v11
* New: Task API performance optimizations


## vProtect 4.0 (Nova)

**30.09.2020**

vProtect 4.0 is a brand new major release that optimizes web interface for even more intuitive user experience.

Two new backup strategies for RHV/oVirt and Proxmox significantly widen range of backup strategies available
for administrators with focus on incremental backup enhancements.

Restore process also now allows to automatically power on VMs or automatically customize names of restored
VMs in Recovery Plans. This opens additional test scenarios, in which administrators can validate if VMs restore successfully and do a test power on. 
This release also simplifies Nutanix setup - administrators now also can add Prism Central directly instead of individual Prism Elements. This allows administrators to use categories (corresponding to "tags" in vProtect) and assign policies automatically based them in policies.  
Last, but not least - S3/S3-compatible backup provider can optionally be connected via proxy. This provider
now also supprots Nutanix Objects.

**Changes:**

* New: Proxmox VE - new backup strategy for Proxmox VE (includes incremental backups, FS freeze, and option to share backups over iSCSI)
* New: RHV/oVirt - new backup strategy using CBT (using new RHV/oVirt backup APIs, currently in Technical Preview)
* New: Nutanix AHV – option to connect via Prism Central instead of individual Prism Elements
* New: Nutanix AHV – option to automatically assign VMs to the policies based on Nutanix categories (vProtect tags) when using Prism Central as hypervisor manager
* New: Option to automatically power on VM after restore
* New: Option to automatically generate VM name with prefix/suffix in Recovery Plans
* New: Major Web UI update and reorganization - better arrangment of views related to the artifacts in the same domain, such as Virtual Environment-related schedules, policies etc.
* New: S3/S3-compatitble backup provider - Nutanix Objects support
* New: S3/S3-compatible backup provider - proxy support
* New: Kubernetes/OpenShift - token-based authentication
* New: Task APIs optimizations
* New: Support for Chrome SameSite cookie security flag
* Fix: oVirt/RHV UI plugin minor enhancements and fixes
* Fix: Hyper-V delta snapshot files merge issue


## vProtect 3.9.2 (Quasar)

**01.06.2020**

vProtect 3.9 Update 2 is a provides multiple - mostly periodic, security-related - updates of internal database, API and web UI.

This  update also introduced CentOS/RHEL 8 support for vProtect deployments. OpenStack and OpenShift environments using Ceph storage can also be now protected - incremental backups as well.

OpenShift backups allow also to perform pre/post export remote command execution on the deployment (which are useful to trigger i.e. DB quiesce/resume operations).

One also can notice restore history visible in UI (both on a single VM/app level as well as in Recovery Plan details view).

All restore jobs are also now reported in e-mail and you can browse them from the dashboard.
Installation playbooks now also allow administrators to specify alternative MariaDB repository location and version.

**Changes:**

* New: restore history in reporting (dashboard and report mail)
* New: restore history in Recovery Plans
* New: OpenStack - disk attachment with Ceph-based deployments - incremental backups (technical preview)
* New: OpenShift - Ceph-backed PVs support - full and incremental backups with RBD-NBD (technical preview)
* New: OpenShift - pre/post export remote command execution
* New: CentOS/RHEL 8 support
* New: Ansible-based installation with option to customize MariaDB source, version and distribution 
* Update: vProtect server security updates in API and UI layers
* Update: MariaDB 10.4 in new deployments


## vProtect 3.9.1 (Quasar)

**10.02.2020**

vProtect 3.9 Update 1 introduces several enhancements for OpenStack and KVM stand-alone environments.
This release introduces new strategy for OpenStack, which uses Cinder-based disk-attachment to perform backups.

For KVM stand-alone vProtect now supports LVM thin-pools and mixed configurations (i.e. QCOW2+LVM).
This update also introduces Ceph RBD support for stand-alone KVM hypervisors.

With 3.9 Update 1 we also provide oVirt integration, so that you can now invoke backup & restore
operations directly from oVirt/RHV administration interface.

**Changes:**

* New: OpenStack - disk attachment backup strategy using Cinder
* New: KVM stand-alone - LVM thin-pool support
* New: KVM stand-alone - support for VMs with mixed disk types
* New: KVM stand-alone - Ceph support
* New: oVirt/RHV Backup & Restore UI integration for vProtect
* New: UI - configuration wizard enhancements


## vProtect 3.9 (Quasar)

**28.10.2019**

vProtect 3.9 is a major release adding officially support for VMware and Hyper-V virtualization platforms.
Both full and incremental backups are supported and it also allows you to restore individual files.
Second huge improvement, for all supported platforms we introduce Recovery Plans. Now, you'll be able
to define rules, how and where multiple VMs should be restored when you need DR. You also are able
to schedule them and restore set of VMs periodically to your test locations, to be sure that you're able
to restore them if you need to.

Several improvements are related to Application backup. Quasar release introduced several build-in
application backup templates for common use cases ready to be used. You also are now able to parametrize
you command execution configurations to force set of parameters that are required for you command or script.
Application restore will also provide several option how to cope with automatic archive extraction or overwrite existing files.

For Amazon users we also have good news - now S3 backup provider allows to enable extended retention
for stored backups and change storage class for oldest objects to Glacier giving you option to reduce your storage costs significantly. 

RHV/oVirt/OLVM users can also boost their backups with netcat transfer and for SSH transfer mode,
restore operations now use direct connection instead of Disk Image Transfer API.
This release also improves Web UI especially in configuration related activities, adds restore history and
improves task console and many more.

**Changes:**

* New: VMware - official release - significant changes in integration layer 
* New: VMware - restore - target disk format selection
* New: VMware - added Hot-Add transfer option if available
* New: VMware - support for tag-based policy auto-assignment
* New: Hyper-V - full/incremental backup
* New: Hyper-V - snapshot management
* New: Hyper-V - file-level restore with mountable backups
* New: Hyper-V - option to share drives over iSCSI
* New: Recovery Plans - batch VM restore for DR
* New: Recovery Plans - periodic VM restore for DR tests
* New: Mountable backups - option to mount QCOW2 with NTFS file systems
* New: Restore operation - option to delete VM (if one already exists with the same name)
* New: Application backup - command execution configuration parameters
* New: Application backup - templates and ready scripts for several common use cases
* New: Application backup - option to clone applications and command execution configurations
* New: Application backup - automatic archive extraction and option to overwrite
* New: Application backup - option to ignore error codes or standard error output for user-provided scripts
* New: OpenStack - Ceph-based storage support
* New: RHV/oVirt/OLVM - netcat transfer option
* New: RHV/oVirt/OLVM - SSH transfer - restore operation now also over SSH
* New: Swift - compression support
* New: Dell-EMC Avamar support
* New: AWS Glacier support
* New: Storware Insight - detailed backup status to reporting option
* New: Ansible playbooks for installation
* New: UI - configuration wizard
* New: UI - task console improvements
* New: UI - global search for quick access to any item
* New: UI - restore jobs history


## vProtect 3.8 Update 1 (Nebula)

**19.06.2019**

vProtect 3.8 Update 1 introduces 3rd backup strategy for RHV/oVirt 4.2+ environments. Using direct SSH transfer from hypervisors vProtect will be able to perform backups significantly faster. All of the features that currently vProtect supports for oVirt 4.2 are now also available
for Oracle Linux Virtualization Manager.
 Another big update is OpenStack support. Currently for environments with KVM VMs  using QCOW2 disks. We also provided Horizon integration to allow more seamless integration.
For Oracle VM we introduced option to exclude drives. This version also updates our encryption mechanism for Azure and File System backup destinations to support large VM backups. Update 1 also provides minor
UI improvements and fix for "snapshot ID not found in the DB" if full backup has not been done yet.

**Changes:**

* New: RHV/oVirt - new "SSH transfer" export/import mode
* New: Oracle Linux Virtualization Manager support - same feature set as for oVirt 4.2
* New: OpenStack - full/incremental backup support
* New: OpenStack - file-level restore support
* New: OpenStack - Horizon integration
* New: Oracle VM - disk exclusion support
* New: Kubernetes - support for containerd
* New: Web UI - improvements in batch hypervisor operations like node/password modification
* New: Web UI - improved backup status reporting
* Update: updated Azure/file system encryption mechanism to support 64+ GB backups
* Fix: automatic full backup if snapshot ID for incremental backup has not been recorded yet


## vProtect 3.8 (Nebula)

**11.04.2019**

vProtect 3.8 (Nebula) is a massive update. It introduces Amazon EC2 backup support with snapshot-management and file-level restore capabilities. Proxmox users will have multiple additional features including automatic backup import to the Proxmox hypervisor, disk-exclusion, Snapshot-management and pre/post snapshot remote command execution. You also will be able to store your backups in Google Cloud Storage.

In Nebula release we enhanced mounted backup capabilities. Now you can browse and restore backup files directly in Web UI, or share RAW-based backups over iSCSI and mount them directly in your target system (especially useful when you want to preserve Windows file permissions). RAW-based backups with NTFS file systems can now be mounted automatically as well (previously it was only manual mount).

Web UI has been improved with new list views with improved paging and filtering. Logs from remote logs can now are automatically shared with the server so that you can browse them in Web UI or download all of them with a single click. New languages - German and Spanish - are also available.

Version 3.8 also improved restore options. Citrix XVA-based backups are now restored as a VM automatically, RHV backups can have allocation type specified and for OpenShift/Kubernetes we added fix for „restore to the specific project” option.

Last, but not least, we updated our application server and data access layer and added option to automatically send vProtect logs to Storware Insight
service.

**Changes:**

* New: Amazon EC2 - full backup with disk-exclusion support - for EC2-based VMs
* New: Amazon EC2 - file-level restore with mountable backups
* New: Amazon EC2 - snapshot/management
* New: Proxmox - automatic backup import to the Proxmox hypervisor
* New: Proxmox - file-level restore with mountable backups
* New: Proxmox - snapshot management
* New: Proxmox - disk exclusion for backups
* New: Proxmox - pre/post snapshot remote command execution (post-export for VMs, post-snapshot for containers)
* New: Google Cloud Storage support
* New: Mounted backups - option to share RAW disks over iSCSI (for platforms that use RAW format in backups)
* New: Mounted backups - file-level restore directly from Web UI
* New: download-all logs feature in Web UI
* New: option to browse logs from remote nodes in Web UI - now mounted automatically
* New: Citrix - XVA restored as a VM
* New: NTFS auto-mount (for RAW-based backups)
* New: RHV - sparse/preallocated format option for restore
* New: German and Spanish language support in Web UI
* New: Storware Insight - automatic logs upload
* Update: Payara 5 pplication Server and data access layer
* Fix: Kubernetes/OpenShift - fixed restore to the specific project option


## vProtect 3.7 Update 2 (Multiverse)

**28.02.2019**

vProtect 3.7 Update 2 adds support for OpenShift. Now you'll be able to use
the same backup methodology as for Kubernetes to protect your persistent volumes on
OpenShift platform. 

We also updated our sample CloudForms integration to match current vProtect API level.

**Changes:**

* New: OpenShift - support for full backup of persistent volumes and deployment configuration
* Update: CloudForms integration matching 3.7.x API.


## vProtect 3.7 Update 1 (Multiverse)

**21.12.2018**

vProtect 3.7 Update 1 extends Multiverse release with additional snapshot management capabilities, 
such as option to revert snapshots directly from UI and adds support for Nutanix VMs. Moreover,
application backup now allows you to define environment variables for each application. This
gives more flexibility when reusing same scripts for multiple instances of the same application.

This update also introduces Chinese language support in UI, as well provides further CLI and backup destination improvements. These include ability to restore backup chains residing on multiple backup destinations and gives additional option for file systems where random access may cause issues when reading data randomly when using mounted backups.

**Changes:**

* New: Snapshot management - Nutanix AHV
* New: Revert snapshots from vProtect console - RHV/oVirt, Citrix XenServer, Nutanix AHV
* New: Application backup - environment variables support
* New: UI - Chinese language supported
* New: updated RedHat CloudForms integration to match 3.7 APIs
* New: option to disable random access for FS backup destinations (which may be essential for file systems such as LTFS)
* New: CLI - backup time details
* New: option to restore backup chains residing on multiple backup destinations
* Fix: arguments in pre/post snapshot command, pre/post BD access command were forced to be unique


## vProtect 3.7 (Multiverse)

**30.10.2018**

vProtect 3.7 (Multiverse) introduces many significant changes. First of them, and the reason why we given such code name was snapshot management. Now you can have state of your environments being periodically
saved without exporting backup itself. This comes together with new interval-based schedules that allow you
to snapshot VM i.e. every hour.

"Multiverse" also relates to multiple platforms that we support and new possible sources of backup.
That is basic backup/restore capabilities for Kubernetes as well as generic approach to backup any application that you can provide us script for. This means that you should be able to cover use cases such as database or hypervisor backup directly using vProtect. A good example is that vProtect DB now can be setup as an "application" that is being backed up using the settings directly from UI.

What is more we have provided way to deploy vProtect using Dockerfiles, introduced support for Oracle VM tags in auto-assignment of VMs, and made significant changes in UI - especially related to the concept of Policies.

**Changes:**

* New: Snapshot Management - for RHV/Citrix/KVM
* New: KVM - incremental backup support
* New: KVM - RAW file (.img) VM disk support
* New: Kubernetes support - backup and restore of Persistent Volumes used in Kubernetes Deployments
* New: Application backup - generic mechanism to backup any application using custom commands
* New: automatic vProtect database backup
* New: Oracle VM - support for tags (VM auto-assignment)
* New: vProtect deployment using Dockerfiles
* New: VM Backup Policies replaced VM groups
* New: Interval-based schedules
* New: UI - non-present VMs cleanup button
* Fix: Schedule/VM group checkboxes race-condition - now working also in Firefox


## vProtect 3.6 (Stardust)

**26.07.2018**

Stardust release enhances file system backup destination with VDO deduplication.
One can say that VDO literally "turns data into dust" and can be enabled directly from UI.
KVM-standalone users will also benefit with pre/post snapshot command execution
and ability to import VM back to the hypervisor. As the GDPR is already effective, 
we implemented encryption option for S3, Azure and file system backup destinations. 

vProtect 3.6 also introduces also many smaller (resembling stardust again), 
but long-awaited features such as LDAP authentication, detailed time statistics
for backups, improved dashboard, or VM auto-assignment based on hypervisor clusters.

**Changes:**

* New: File system backup destination – deduplication with VDO
* New: FS/S3/Azure backup destinations – data encryption
* New: LDAP authentication
* New: RHV/oVirt/KVM/Xen/XenServer - option to set restored VM name
* New: RHV/oVirt optimized snapshot for selected disks only
* New: KVM/Xen – LVM VG scanning (Hypervisor Storage)
* New: VM auto-assignment based on cluster
* New: Web UI – improved dashboard
* New: Web UI – additional backup statistics: size and time in VM details
* New: other UI improvements
* Fix: Server-Node communication optimizations


## vProtect 3.5 (Parsec)

**30.05.2018**

vProtect 3.5 introduces support for incremental backups of RHV/oVirt environments.
With the new Disk Image Transfer mode, administrators will be able to protect
their environments without the need to use export storage domain or Proxy VM.

New vProtect also does additional storage space checking (with configurable thresholds),
to protect virtualization platform storage to be filled up.

With pre/post snapshot mechanism administrators will be able to quiesce services
before snapshot backup with their own custom scripts to make backups application-level
consistent.

Similarly, it is also possible now to customize mounting volumes before it is accessed
dynamically with backup destination pre/post access command execution. A good example
of its usage is Catalogic vStor Server support, including backups being replicated to the remote
location. 

Version 3.5 also introduces support for Microsoft Azure as another cloud-based option
for storing backups.

**Changes:**

* New: RHV/oVirt 4.2+ - Disk Image Transfer backup mode (incremental backup support)
* New: RHV/oVirt/Citrix/Nutanix - pre/post snapshot remote command execution
* New: RHV/oVirt/Citrix/Nutanix - available storage check before snapshot creation
* New: pre/post backup destination access custom command execution
* New: Microsoft Azure backup destination
* New: Catalogic vStor Server support (with replication)
* New: logging improvements for backup destinations
* Fix: UI node reassignment failed for HV/HVM after original has been removed


## vProtect 3.4 (Mars)

**06.04.2018**

vProtect 3.4 introduces API v4 support for oVirt/RHV environments, which comes together
with a new backup strategy available. This means that vProtect Node can now act as
a proxy VM, without the need of using export storage domain and a cloning step.

This release also introduces cluster and storage scanning to allow administrators to easily
select destination storage from the list in restore dialog box. Moreover, new vProtect version
also extends scheduling options to allow administrators to configure backups to be done
on monthly or even annually basis. 

Last but not least, vProtect now can fail remaining backup tasks in the queue if it detects that
certain percent of backup tasks have already failed. This helps administrator to address
cases when unavailability of backup provider or VM infrastructure causes multiple backups
to constantly fail during scheduled backup process.

**Changes:**

* New: RHV/oVirt API v4 support - backup without export storage domain
* New: extended scheduling options
* New: Cluster and storage indexing (RHV/oVirt/OVM/Nutanix/XenServer)
* New: automatic stop of scheduled backup if more than X % of backups already failed
* Fix: backup retention settings could result in full backups being kept longer than configured
* Fix: Nutanix - backup of VMs with mounted ISOs resulted in error


## vProtect 3.3 (Warp)

**01.03.2018**

vProtect 3.3 introduces support for Nutanix AHV hypervisors. It is also able
to create incremental backups using Nutanix CRT (CBT) feature, and provides
selective disk backup for this platform.

For Citrix hypervisors vProtect now allows to set transfer NIC used to transfer backups.
Backups of VMs running on KVM/Xen (stand-alone) hypervisors can be performed
with exclusion of certain drives.

This release also provides updated application server, including multiple security fixes
and code optimizations. Moreover initial configuration of the node has been scripted
to make the process easier.

**Changes:**

* New: Nutanix AHV support including CBT for incremental backups
* New: Nutanix AHV - file-level restore
* New: Nutanix AHV - disk exclusion
* New: KVM (stand-alone) and legacy Xen - disk exclusion
* New: Citrix - option to specify transfer NIC for hypervisor
* New: NetBackup - option to access the same NB server from multiple nodes
* New: e-mail reports with VM grouping
* New: updated application server
* New: scripted OS preparation for node
* Fix: Networker - empty versions list caused exception
* Fix: additional code optimizations and fixes 


## vProtect 3.2.3 (Gravity)

**16.02.2018**

vProtect 3.2.3 release fixes incremental backup related issues, which are mostly related to Citrix CBT backup implementation. It also adds S3 DNS based round-robin support for local object storage installations and provides minor UI fixes and improvements.

**Changes:**

* Fix: Citrix - CBT - retention handling for incremental backups
* Fix: Citrix - CBT backups had wrong relationship recorded
* Fix: Citrix - session management problem during incremental backup import
* Fix: UI - selecting incremental backup in backup window resulted in error being shown
* Fix: prevent schedules to be executed if they have just been created and time passed
(tasks would be expired)
* New: UI - add Node Config selection in Backup Destination create/edit forms
* New: Citrix - restart CBT on all VHDs during full backup
* New: S3 backup destination - if DNS name is used in endpoint URL, try to resolve and 
reconnect, so that DNS Round-Robin can be effective (third party S3 API)


## vProtect 3.2.2 (Gravity)

**22.01.2018**

vProtect 3.2.2 release adds new settings for third party S3 API implementations (i.e. Scality)
It also fixes minor bugs in UI and CLI.

**Changes:**

* Fix: UI - filtering - select all option was not using filter settings
* Fix: CLI - incorrect name used while creating backup destination
* New: S3 backup destination option - record backup time after storing object (third party S3 API)
* New: S3 backup destination mode - single bucket with disabled versioning (third party S3 API)


## vProtect 3.2.1 (Gravity)

**15.01.2018**

vProtect 3.2.1 release is focused on UI fixes and enhancements.
Administrators will now be able to backup whole VM group with just one click.
A new chart in the VM details makes also easier to keep track of backup size.

**Changes:**

* Fix: UI - missing tooltips
* Fix: UI - drop-down lists arrows handling
* Fix: logo in e-mail reports was not rendered properly in Outlook
* Fix: empty backup log download handling
* New: CLI - node registration in batch mode
* New: VM backup size chart
* New: VM group backup
* New: Task console full screen mode and task counters
* New: other UI improvements (additional info in forms, filtering)


## vProtect 3.2 (Gravity)

**22.12.2017**

vProtect 3.2 adds new backup capabilities for Citrix environments.

Now you will be able to use XenServer 7.3 Changed Block Tracking feature 
to boost your incremental backups. vProtect is now also able to mount Citrix 
backups on the vProtect Nodes without the need to restore it to the XenServer. 
One can also exclude selected VM disks from backup (Citrix).

Proxmox users will now also be able to configure compression of their backups. 
For RHV/oVirt environments. This release also adds option to freeze filesystems
before snapshot creation. 

This release also optimizes integration related functions used by backup providers, 
adds instant notifications for failed backups and provides multiple enhancements
in the web UI.

**Changes:**

* New: Changed Block Tracking support for XenServer 7.3+
* New: option to exclude selected VM disks from backup (Citrix)
* New: file-level restore for Citrix XenServer
* New: file-level restore for KVM (stand-alone) and Xen (legacy)
* New: quiesced-snapshot (Citrix)/file system freeze before snapshot (RHV/oVirt) option on VM level
* New: instant notification about recently failed backups
* New: UI enhancements and optimizations
* New: compression options for Proxmox
* New: backup provider integration optimizations


## vProtect 3.1.5 (New Horizons)

**15.11.2017**

vProtect 3.1.5 fixes several task handling issues, including timeout settings
not being used properly used. Node removal also removes mounted backups
and HV/HVM type fields are kept even when VM is removed.

**Changes:**

* New: node stacktraces logging enhancements
* Fix: timeouts in settings were not used when creating a task (default 1h always assigned)
* Fix: store task now cleans up files on expiration
* Fix: node removal should remove mounted backups
* Fix: HV/HVM type field should not be cleared when VM no longer exists


## vProtect 3.1.4 (New Horizons)

**06.11.2017**

vProtect 3.1.4 introduces enhanced auto-assign feature. Now users can
add/remove automatically VMs based on set of tags and regular expression
using both include and exclude rules. There are also few UI related bug fixes.

**Changes:**

* New: RHV/oVirt/Citrix - include/exclude VMs automatically to/from groups 
using set of regular expressions or tags
* Fix: backup priority is always 50 in web UI
* Fix: S3 keys could not be saved in web UI
* Fix: error handling when backup is not found in backup destination during restore


## vProtect 3.1 (New Horizons)

**10.10.2017**

Release 3.1 is a completely new vProtect. Brand new UI, CLI, open API, app server and database.
It introduces also support for Proxmox hypervisors. With 3.1 you can now scale
horizontally with multi-node centralized management and store backups in different backup destinations.


New Horizons release introduces also RPM based installation and upgrade, XVA compression (Citrix)
and several security enhancements.

**Changes:**

* New: Web UI
* New: open API for 3rd-party integration
* New: multi-node deployments
* New: multiple backup destinations - now they can be used simultaneously
* New: database changed to MariaDB and embedded application server
* New: refreshed CLI
* New: RPM based installations and updates
* New: Proxmox support
* New: Citrix XVA compression
* New: user management
* New: security enhancements


## vProtect 2.6.4 (Pulsar)

**10.08.2017**

Release 2.6.4 fix solves problem with XenServer environments when multiple
hypervisors have same SR name (which results in import issues). Now users
can pass SR UUID to uniquely identify target SR for import. This release also
includes DB optimizations and introduces move operation in file system BP
for vProtect environments using same file system for staging and as a storage
for stored backups.

**Changes:**

* Fix: Citrix XenServer - SR UUID can now be passed as SR name for import operation
(when multiple SRs are with the same name)
* Fix: DB optimizations
* New: File System backup provider moves file if it is on the same FS (store operation)


## vProtect 2.6.3 (Pulsar)

**11.07.2017**

Release 2.6.3 fix solves problem with Oracle VM environments with VMs that
use different storage repositories for config and VM disks. It also provides
minor enhancement in exception logging and support for SuSE Linux in installer.

**Changes:**

* Fixed: Oracle VM - failed export when VM uses multiple storage repositories
* Fixed: minor installation script fixes
* New: SUSE Linux support in installer
* New: extended logging for hypervisor related exceptions


## vProtect 2.6.2 (Pulsar)

**27.06.2017**

Release 2.6.2 provides workaround for RHEV 3.5.x API which doesn’t provide all information needed
to automatically select appropriate export domain for datacenters. It also fixes NetWorker restore
operation (which previously did not bring original paths after files have been restored).

**Changes:**

* Fixed: optional setting - storage domain to datacenter mapping in settings
* Fixed: NetWorker restored files to wrong location, which resulted in mount failure


## vProtect 2.6.1 (Pulsar)

**13.06.2017**

Release 2.6.1 fixes several issues regarding removal of task and backup objects from the DB
and addresses old RHEV API issue related to storage domains. It also fixes S3 timestamp mismatch
problem, which may result in failed backups.

**Changes:**

* Fixed: task removal failure in some situations
* Fixed: RHEV 3.6 - old API did not provide info about export domains
* Fixed: backup removal fixed
* Fixed: S3 wrong timestamp in the DB


## vProtect 2.6 (Pulsar)

**31.05.2017**

With release 2.6 we have introduced file-level restore for RHV/oVirt and Oracle VM environments.
Together with possibility to stream backup directly from file system backup provider, now users can 
almost instantly access specific files in VM backups. 

Citrix and RHV/oVirt environments also received tag-based auto-assignment feature to simplify
process of grouping VMs and scheduling backups.

In this version we also added Dell-EMC Networker support, which means that currently users have
option to store backups in 3 enterprise-grade backup providers.

Amazon S3 backup provider also has been enhanced with secondary mode - one bucket for all data,
which is required by some vendors using S3 protocol. This means that vProtect can now also use
IBM Cleversafe as the backup provider.

**Changes:**

* New: Dell-EMC Networker support
* New: IBM Cleversafe support
* New: Amazon S3 - single bucket mode
* New: File-level restore with mountable backups (RHV/oVirt and Oracle VM)
* New: VM auto-assignment based on tags (Citrix XenServer and RHV/oVirt)
* New: Backup size in Virtual Machines view (web UI)
* Updated: Keep last backup locally option - optimizations + support for all hypervisor types
* Updated: Improved installer


## vProtect 2.5 (Trappist)

**16.03.2017**

The key feature of this version is support for incremental backups (Citrix XenServer). vProtect is now also able to restore VMs directly to XenServer hypervisors. With added support for Amazon S3 as a backup provider one can also store backups directly in the cloud. Last but not least, version 2.5 includes several updates and bug fixes in the backup providers’ libraries and multiple optimizations in engine’s code.

**Changes:**

* New: Incremental backup (Citrix XenServer)
* New: Restore to hypervisor (Citrix XenServer)
* New: Amazon S3 backup provider
* New: Retention settings managed by vProtect
* Updated: TSM API library, which includes multiple fixes and improvements
* Updated: OpenStack Swift API library
* Updated: improved installer


## vProtect 2.4.1

**30.01.2017**

This is a bugfix release focused on DB mapping issues when hypervisors were removed.
It also verifies which backup providers are covered by license and fixes minor license-related issues.

**Changes:**

* Fixed: removal of hypervisors, hypervisor managers and VM groups now is properly reflected in foreign keys in DB
* Fixed: verbose message if license is not valid when starting engine
* Fixed: license verification for backup providers
* Fixed: configuration refresh before starting the engine


## vProtect 2.4

**11.01.2017**

Main improvements compared to 2.3:

1. **File System as the backup provider**, which means that:
  
  * you can use any mounted set of file systems (mount points) as the storage space
  * especially those can have deduplication capabilities like DD Boost (Data Domain) or OpenDedup
  * then you can use them as the destination for your backups (retention settings: 
N last versions and keep not older than N days backup - same as for Swift)
  * technically, you can use ANY mountable file system, even remote (or set of file systems)
  * vProtect will balance the usage based on the amount of free space

1. Small but important - **auto-assignment of VMs to groups** 
  * I’ve heard about this requirement several times from the customers
  * Index task will try to match name agains regular expressions and assign to the group
Just a short reminder, cause I think I haven’t summarize 2.3 (previous) version to you:
  * added OracleVM support
  * added Veritas NetBackup as the backup provider

This means that there is already quite a lot of supported virtualization platforms
and possible backup destinations.


## vProtect 2.3

**19.12.2016**

* added OracleVM support
* added Veritas NetBackup as the backup provider


## vProtect 2.2

**16.05.2016**

A brand new release - vProtect 2.2 - extends range of supported platforms.

With vProtect 2.2 you can now easily implement backup of virtual machines
running in **Red Hat Enterprise Virtualization and oVirt** environments.

Moreover, you can now store you backups both in IBM Spectrum Protect
and in **Openstack Swift object storage**. With just a few clicks you can connect
to your existing Swift environment and set retention policy for your VM backups.

In vProtect 2.2 we also significantly improved engine and enabled dynamic
reconfigurations directly from web interface. 


## vProtect 2.1

**18.02.2016**

* Added Web UI
* KVM/Xen support (full backup only)


## vProtect 1.0

**30.06.2015**

* Basic CLI
* backup/restore Citrix XenServer (restore to node’s file system only)
* TSM support

