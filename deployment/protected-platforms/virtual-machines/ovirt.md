# oVirt

## General

For oVirt 4+ environments you can use API v4 for invoking all backup related tasks.

Import/export mode defines the way the backups and restores are done. oVirt \(with API v4\) supports 3 modes:

1. **Disk attachment**, which exports VM metadata \(in OVF format\) with separate disk files \(in RAW format\) via Proxy VM with the Node installed.
   * supports oVirt 4.0+
   * no incremental backup
   * proxy VM required in each cluster - used for disks attachment process
2. **Disk image transfer**, which exports VM metadata \(in OVF format\) with disk snapshot chains as separate files \(QCOW2 format\):
   * supports oVirt 4.2+/oVirt 4.2.3+
   * supports incremental backup
   * disk images are transferred directly from API \(no Proxy VM required\)
3. **SSH Transfer,** this method assumes that all data transfers are directly from hypervisor - over SSH
4. **Change Block Tracking,** this method backup only blocks with changes and skips zeroed sectors.
   * supports oVirt 4.4+ \(with Libvirt 6+, qemu-kvm 4.2+ and vdsm 4.40+\)
   * supports incremental backup
   * only disks with marked "enable incremental backup" in ovirt will be backuped

When adding oVirt 4.0+ hypervisor managers use a URL similar to the following:

```text
https://oVirt_MGR_HOST/ovirt-engine/api
```

**Note:** username for oVirt environments needs to be provided in **user@domain** format - i.e. **admin@internal**. This user must have all permissions related to manage snapshots, create/remove VMs, operate disks and export data.

## Backup Strategies

oVirt environments can be protected in several ways.

**Note:** Different strategies require a node to be installed either as a VM on the environment that you backup or installed separately.

### Disk attachment with Proxy VM

In this strategy, you have a VM called “Proxy VM” that invokes commands on your hypervisor manager to snapshot and attach drives of a specific VM to itself \(Proxy VM\). Proxy VM is able to read the data from the attached disk snapshots and forward them to the backup provider.

This strategy allows you to exclude disks that you don't need from your backup. Remember that, you need to install 1 Proxy VM per cluster so that drives that the node tries to attach to are reachable.

Drawback - no incremental backup for now.

![](../../../.gitbook/assets/deployment-vprotect-ovirt-disk-attachment.png)

**Note**: oVirt API v4 environments require vProtect Node to be installed in one of the VMs residing on the oVirt cluster. vProtect should detect automatically the VM with vProtect during index operation.

Disk attachment mode requires `Virtio-SCSI` to be enabled on the vProtect Node VM:

During the backup/restore operations, disks are transferred by attaching them to the proxy VM. This approach does not require export storage domain to be set up.

Please make sure to follow these steps: [LVM setup on vProtect Node for disk attachment backup mode](../../common-tasks/lvm-setup-on-vprotect-node-for-disk-attachment-backup-mode.md).

### Disk image transfer API

This API appeared in oVirt 4.2 and allowed export of individual snapshots directly from the oVirt manager. So, instead of having to install multiple Proxy VMs, you can have a single external Node installation, which just invokes APIs via oVirt manager.

This strategy supports incremental backups. Assuming you have oVirt 4.2 or newer – just add your manager to vProtect and setup is done. From a network perspective - it requires two additional ports to open 54322 and 54323 and your data to be pulled from the hypervisor manager.

Unfortunately, there are a few problems with the current architecture of this solution. The biggest issue is that all traffic passes via oVirt manager, which may impact transfer rates that you can achieve during the backup process. To put that into perspective – in disk attachment you can basically read data as if it is a local drive, where it could potentially be deduplicated even before transferring it to the backup destination.

![](../../../.gitbook/assets/deployment-vprotect-ovirt-disk-image-transfer.png)

Disk image transfer mode exports data directly using oVirt 4.2+ API. There is no need to setup export storage domain or setup LVM. This mode uses snapshot-chains provided by oVirt.

You may need to open communication for additional port **54323** on the oVirt manager - it needs to be accessible from vProtect Node. Also, make sure that your **ovirt-imageio-proxy** services are running and properly configured \(you can verify it by trying to upload images with oVirt UI\).

Follow the steps in this section: [Full versions of libvirt/qemu packages installation](../../common-tasks/full-versions-of-libvirt-qemu-packages-installation.md).

### SSH transfer

This is an enhancement for the disk image transfer API strategy. It allows vProtect to use oVirt API v4.2+ \(HTTPS connection to oVirt manager\) only to collect metadata. Backup is done over SSH directly from the hypervisor \(optionally using netcat for transfer\), import is also using SSH \(without netcat option\). No need to install a node on the oVirt environment. This method can significantly boost backup transfers and supports incremental backups.

![](../../../.gitbook/assets/deployment-vprotect-ovirt-ssh-transfer.png)

This method assumes that all data transfers are directly from the hypervisor - over SSH. This means that after adding oVirt manager and detecting all available hypervisors - **you need to also provide SSH credentials or SSH keys for each of the hypervisors**. You can also use [SSH public key authentication](red-hat-virtualization.md).

### Change Block Tracking

This is a new method which is possible thanks to changes in oVirt 4.4. It uses information about zeroed and changed blocks to reduce data size and make the process faster.

![](../../../.gitbook/assets/vprotect_ovirt-cbt.jpg)

This strategy supports incremental backups.

Qcow2 format is required for incremental backups so disks enabled for incremental backup will use qcow2 format instead of raw format.

Also, this strategy doesn't need snapshots in the backup process. Instead of it, every incremental backup uses a checkpoint that is a point in time that was created after the previous backup.

### Export storage domain \(API v3\)

This setup requires you to create storage domain used for VM export. Export storage domain should accessible also by vProtect Node in its staging directory. This implies that storage space doesn't have to be exported by vProtect Node - it can be mounted from external source. The only requirement is to have it visible from both oVirt host and Node itself. Keep in mind that ownership of the files on the share should allow both vProtect and oVirt to read and write files.

Backup process requires that once the snapshot is created it will be cloned and exported \(in fact to the vProtect Node staging\). The reason for additional cloning is that oVirt doesn’t allow you to export snapshot directly. Node can be outside of the environment that you backup.

This strategy is going to be deprecated, as oVirt may no longer support it in the future releases.

![](../../../.gitbook/assets/deployment-vprotect-ovirt-export-storage-domain.png)

oVirt 3.5.1+ environments \(using API v3\) require export storage domain to be set up.

1. Add backup storage domain in the oVirt \(which points to the NFS export on vProtect Node\)
   * If you have multiple datacenters you need to enable the`Multi DC export` checkbox in node configuration
     * Remember that you need to use named datacenters in your oVirt enviornment to avoid name conflicts
     * oVirt datacenter may use only one export storage domain, that is why you need to create subdirectories for each data center in export path i.e. `/vprotect_data/dc01`, `/vprotect_data/dc02`, and use each subdirectory as NFS share for each data center export domain \(separate NFS exports\)
     * Export \(staging\) path in above-mentioned scenario is still `/vprotect_data`, while `dc01` and `dc02` are datacenter names
     * Older versions of oVirt \(3.5.x\) require to specify mapping between DC names and export storage domains - you need to provide pairs of DC name and corresponding SD name in node configuration \(section `Hypervisor`\)
   * If you have only one datacenter and don't want to use multiple datacenters export feature in the future, you can use default settings, and setup NFS export pointing to the staging path \(e.g. `/vprotect_data`\)
   * Note that export must be set to use UID and GID of `vprotect` user
   * Example export configuration in `/etc/exports` to the selected hypervisor in oVirt cluster:

     ```text
     /vprotect_data    10.50.1.101(fsid=6,rw,sync,insecure,all_squash,anonuid=993,anongid=990)
     ```

     where `anonuid=993` and `anongid=990` should have correct UID and GID returned by command:

     ```text
     [root@vProtect3 ~]# id vprotect
     uid=993(vprotect) gid=990(vprotect) groups=990(vprotect)
     ```
2. Both import and export operations will be done using this NFS share – restore will be done directly to this storage domain, so you can easily import backup into oVirt \(shown below\)
   * backups must be restored to the export path \(node automatically changes names to the original paths that are recognized by oVirt manager.
3. When adding oVirt 4.0+ hypervisors managers make sure to have URL like the following:

   ```text
   https://oVirt_MGR_HOST/ovirt-engine/api/v3
   ```

