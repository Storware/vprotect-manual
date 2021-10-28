# oVirt

## General

For oVirt 4+ environments you can use API v4 for invoking all backup related tasks.

Import/export mode defines the way the backups and restores are done. oVirt \(with API v4\) supports 3 modes:

1. **Disk attachment**, which exports VM metadata \(in OVF format\) with separate disk files \(in RAW format\) via Proxy VM with the Node installed.
   * supports oVirt 4.0+
   * no incremental backup
   * proxy VM required in each cluster - used for the disk attachment process
2. **Disk image transfer**, which exports VM metadata \(in OVF format\) with disk snapshot chains as separate files \(QCOW2 format\):
   * supports oVirt 4.2+/oVirt 4.2.3+
   * supports incremental backup
   * disk images are transferred directly from API \(no Proxy VM required\)
3. **SSH Transfer,** this method assumes that all data transfers are directly from the hypervisor - over SSH
4. **Change Block Tracking,** this method backs up only blocks with changes and skips zeroed sectors.
   * supports oVirt 4.4+ \(with Libvirt 6+, qemu-kvm 4.2+ and vdsm 4.40+\)
   * supports incremental backup
   * only disks marked with "enable incremental backup" in ovirt will be backed up

**Note:** When using backup APIs - Red Hat highly recommends to update oVirt environment to the most recent version (4.4 - at the time of writing) - please refer to this [article](https://access.redhat.com/articles/6379751) for more information.

When adding oVirt 4.0+ hypervisor managers, use a URL similar to the following:

```text
https://oVirt_MGR_HOST/ovirt-engine/api
```

**Note:** a username for oVirt environments needs to be provided in the **user@domain** format - i.e. **admin@internal**. This user must have all permissions related to managing snapshots, creating/removing VMs, operating disks and exporting data.

## Backup Strategies

oVirt environments can be protected in several ways.

**Note:** Different strategies require a node to be installed either as a VM on the environment that you back up or installed separately.

**Note:** All live snapshots are attempted with quiescing enabled. If the snapshot command fails because there is no compatible guest agent present, the live snapshot is re-initiated without the use-quiescing flag.

### Disk attachment with Proxy VM

In this strategy, you have a VM called “Proxy VM” that invokes commands on your hypervisor manager to snapshot and attach drives of a specific VM to itself \(Proxy VM\). Proxy VM is able to read the data from the attached disk snapshots and forward them to the backup provider.

This strategy allows you to exclude drives from backup that you do not need. Remember that you need to install 1 Proxy VM per cluster so that the drives the node tries to attach are reachable.

Drawback - no incremental backup for now.

![](../../../.gitbook/assets/deployment-vprotect-ovirt-disk-attachment.png)

#### **Backup Process**

* crash-consistent snapshot using hypervisor's API
* optionally FS freeze can be executed before snapshot can be executed \(FS thaw once the snapshot is completed\) if enabled and guest tools installed inside
* optional application consistency using pre/post snapshot command execution
* metadata exported from API
* snapshot disks are mounted one by one to the Proxy VM
* data read directly on the Proxy VM
* incremental backups are \_\*\*\_not supported
* restore creates empty disks on the Proxy VM, imports merged data then recreates VM and reattaches volumes to the target VM

**Note**: oVirt API v4 environments require vProtect Node to be installed in one of the VMs residing in the oVirt cluster. vProtect should automatically detect the VM with vProtect during the index operation.

Disk attachment mode requires `Virtio-SCSI` to be enabled on the vProtect Node VM \(which can be enabled in VM settings -&gt; `Resource Allocation` -&gt; `VirtIO-SCSI Enabled` at the bottom\).

During backup/restore operations, disks are transferred by attaching them to the proxy VM. This approach does not require an export storage domain to be set up.

Please make sure you follow these steps: [LVM setup on vProtect Node for disk attachment backup mode](../../common-tasks/lvm-setup-on-vprotect-node-for-disk-attachment-backup-mode.md).

### Disk image transfer API

This API appeared in oVirt 4.2 and allowed export of individual snapshots directly from the oVirt manager. So instead of having to install multiple Proxy VMs, you can have a single external Node installation, which just invokes APIs via the oVirt manager.

This strategy supports incremental backups. Assuming you have oVirt 4.2 or newer – just add your manager to vProtect and the setup is done. From a network perspective, it requires two additional ports to be opened - 54322 and 54323 - and your data to be pulled from the hypervisor manager.

Unfortunately, there are a few problems with the current architecture of this solution. The biggest issue is that all traffic passes via the oVirt manager, which may impact the transfer rates that you can achieve during the backup process. To put that into perspective – in disk attachment you can basically read data as if it is a local drive, where it could potentially be deduplicated even before it is transferred to the backup destination.

![](../../../.gitbook/assets/deployment-vprotect-ovirt-disk-image-transfer.png)

#### Backup Process

* crash-consistent snapshot using hypervisor's API
* optionally FS freeze can be executed before snapshot can be executed \(FS thaw once the snapshot is completed\) if enabled and guest tools installed inside
* optional application consistency using pre/post snapshot command execution
* supported for oVirt/RHV/OLVM 4.3+
* metadata exported from API
* data transfer initiated on the manager and actual data exported from the hypervisor using imageio API
* incremental backups use the same APIs, but requests for changed blocks only
* last snapshot kept on the hypervisor for the next incremental backup \(if at least one schedule assigned to the VM has backup type set to incremental\)
* restore recreates VM from metadata using API and imports merged chain of data for each disk using imageio API

Disk image transfer mode exports data directly using oVirt 4.2+ API. There is no need to set up an export storage domain or setup LVM. This mode uses snapshot-chains provided by oVirt.

You may need to open communication for the additional port **54323** on the oVirt manager - it needs to be accessible from vProtect Node. Also make sure that your **ovirt-imageio-proxy** services are running and properly configured \(you can verify it by trying to upload images with oVirt UI\).

Follow the steps in this section: [Full versions of libvirt/qemu packages installation](../../common-tasks/full-versions-of-libvirt-qemu-packages-installation.md).

### SSH transfer

This is an enhancement for disk image transfer API strategy. It allows vProtect to use oVirt API v4.2+ \(HTTPS connection to oVirt manager\) only to collect metadata. Backup is done over SSH directly from the hypervisor \(optionally using netcat for transfer\), import is also using SSH \(without the netcat option\). There is no need to install a node in the oVirt environment. This method can significantly boost backup transfers and supports incremental backups.

![](../../../.gitbook/assets/deployment-vprotect-ovirt-ssh-transfer.png)

#### Backup Process

* crash-consistent snapshot using hypervisor's API
* optionally FS freeze can be executed before snapshot can be executed \(FS thaw once the snapshot is completed\) if enabled and guest tools installed inside
* optional application consistency using pre/post snapshot command execution • metadata exported from API
* data transfer via SSH \(optional using netcat\) - the full chain of disk snapshot files for each disk o if LVM-based storage is used, then node activates volumes if necessary to read data o if Gluster FS is used, then disk files are copied directly
* incremental backup export just sub-chain of QCOW2-deltas snapshots since last stored snapshot
* last snapshot kept on the hypervisor for the next incremental backup \(if at least one schedule assigned to the VM has backup type set to incremental\)
* restore recreates VM with empty storage from metadata using API and imports merged data over SSH to appropriate location on a hypervisor

This method assumes that all data transfers are directly from the hypervisor over SSH. This means that after adding oVirt manager and detecting all available hypervisors - **you also need to provide SSH credentials or SSH keys for each of the hypervisors**. You can also use [SSH public key authentication](red-hat-virtualization.md).

### Change Block Tracking

This is a new method which is possible thanks to changes in oVirt 4.4. It uses information about zeroed and changed blocks to reduce data size and make the process faster.

![](../../../.gitbook/assets/vprotect_ovirt-cbt.jpg)

This strategy supports incremental backups.

The QCOW2 format is required for incremental backups, so disks enabled for the incremental backup will use the QCOW2 format instead of the raw format.

Also, this strategy doesn't need snapshots in the backup process. Instead, every incremental backup uses a checkpoint that is a point in time that was created after the previous backup.

### Export storage domain \(API v3\)

This setup requires you to create storage domain used for VM export. The export storage domain should also be accessible to vProtect Node in its staging directory. This implies that storage space doesn't have to be exported by vProtect Node - it can be mounted from an external source. The only requirement is to have it visible from both the oVirt host and the Node itself. Keep in mind that ownership of the files on the share should allow both vProtect and oVirt to read and write files.

The backup process requires that once a snapshot is created it will be cloned and exported \(in fact to vProtect Node staging\). The reason for additional cloning is that oVirt doesn’t allow you to export snapshots directly. The Node can be outside of the environment that you back up.

This strategy is going to be deprecated, as oVirt may no longer support it in future releases.

![](../../../.gitbook/assets/deployment-vprotect-ovirt-export-storage-domain.png)

#### Backup Process

* crash-consistent snapshot is taken via API
* optional application consistency using pre/post snapshot command execution
* initial VM clone of the snapshot to the local repository is created
* cloned VM \(data+metadata\) exported by the manager to the vProtect staging space \(visible as the export Storage Domain in managers UI\)
* full backup only is supported
* restore is done to the export Storage Repository, the administrator needs to import the VM using manager UI

#### How to set up a backup with an export storage domain

oVirt 3.5.1+ environments \(using API v3\) require an export storage domain to be set up.

1. Add a backup storage domain in oVirt \(which points to the NFS export in vProtect Node\)
   * If you have multiple data centers, you need to enable the`Multi DC export` checkbox in node configuration
     * Remember that you need to use named data centers in your oVirt environment to avoid name conflicts
     * An oVirt data center may use only one export storage domain, that is why you need to create sub-directories for each data center in the export path i.e. `/vprotect_data/dc01`, `/vprotect_data/dc02`, and use each sub-directory as NFS share for each data center export domain \(separate NFS exports\)
     * The export \(staging\) path in the above-mentioned scenario is still `/vprotect_data`, while `dc01` and `dc02` are data center names
     * Older versions of oVirt \(3.5.x\) require you to specify mapping between DC names and export storage domains - you need to provide pairs of a DC name and a corresponding SD name in the node configuration \(section `Hypervisor`\)
   * If you have only one data center and don't want to use the multiple data centers export feature in the future, you can use the default settings and setup NFS export pointing to the staging path \(e.g. `/vprotect_data`\)
   * Note that the export must be set to use the UID and GID of `vprotect` user
   * Example export configuration in `/etc/exports` to a selected hypervisor in the oVirt cluster:

     ```text
     /vprotect_data    10.50.1.101(fsid=6,rw,sync,insecure,all_squash,anonuid=993,anongid=990)
     ```

     where `anonuid=993` and `anongid=990` should have the correct UID and GID returned by command:

     ```text
     [root@vProtect3 ~]# id vprotect
     uid=993(vprotect) gid=990(vprotect) groups=990(vprotect)
     ```
2. Both import and export operations will be done using this NFS share – restore will be done directly to this storage domain, so you can easily import the backup into oVirt \(shown below\)
   * backups must be restored to the export path \(the node automatically changes names to the original paths that are recognized by oVirt manager.
3. When adding oVirt 4.0+ hypervisor managers, make sure you have a URL like the following:

   ```text
   https://oVirt_MGR_HOST/ovirt-engine/api/v3
   ```

