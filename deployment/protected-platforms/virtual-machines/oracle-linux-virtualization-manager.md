# Oracle Linux Virtualization Manager

## General

For Oracle Linux Virtualization Manager \(OLVM\) 4+ environments you can use API v4 for invoking all backup related tasks.

Import/export mode defines the way the backups and restores are done. OLVM \(with API v4\) supports 3 modes:

1. **Disk attachment**, which exports VM metadata \(in OVF format\) with separate disk files \(in RAW format\) via Proxy VM with the Node installed.
   * supports OLVM 4.0+
   * no incremental backup
   * proxy VM required in each cluster - used for the disks attachment process
2. **Disk image transfer**, which exports VM metadata \(in OVF format\) with disk snapshot chains as separate files \(QCOW2 format\):
   * supports OLVM 4.2+/oVirt 4.2.3+
   * supports incremental backup
   * disk images are transferred directly from API \(no Proxy VM required\)
3. **SSH Transfer,** this method assumes that all data transfers are directly from hypervisor - over SSH

When adding OLVM hypervisor managers use a URL similar to the following:

```text
https://OLVM_MGR_HOST/ovirt-engine/api
```

**Note:** username for OLVM environments needs to be provided in **user@domain** format - i.e. **admin@internal**. This user must have all permissions related to manage snapshots, create/remove VMs, operate disks and export data.

## Backup Strategies

OLVM environments can be protected in several ways.

**Note:** Different strategies require a node to be installed either as a VM on the environment that you backup or installed separately.

### Disk attachment with Proxy VM

In this strategy, you have a VM called “Proxy VM” that invokes commands on your hypervisor manager to snapshot and attach drives of a specific VM to itself \(Proxy VM\). The proxy VM is able to read the data from the attached disk snapshots and forward them to the backup provider.

This strategy allows you to exclude drives from a backup that you do not need. Remember that, you need to install 1 Proxy VM per cluster so that drives that the node tries to attach to are reachable.

Drawback - no incremental backup for now.

![](../../../.gitbook/assets/deployment-vprotect-olvm-disk-attachemnt.png)

#### **Backup Process**

* crash-consistent snapshot using hypervisor's API
* optionally FS freeze can be executed before snapshot can be executed \(FS thaw once the snapshot is completed\) if enabled and guest tools installed inside
* optional application consistency using pre/post snapshot command execution
* metadata exported from API
* snapshot disks are mounted one by one to the Proxy VM
* data read directly on the Proxy VM
* incremental backups are ****not supported
* restore creates empty disks on the Proxy VM, imports merged data then recreates VM and reattaches volumes to the target VM

**Note**: OLVM API v4 environments require vProtect Node to be installed in one of the VMs residing on the OLVM cluster. vProtect should detect automatically the VM with vProtect during index operation.

Disk attachment mode requires `Virtio-SCSI` to be enabled on the vProtect Node VM\(which can be enabled in VM settings -&gt; `Resource Allocation` -&gt; `VirtIO-SCSI Enabled` at the bottom\).

During the backup/restore operations, disks are transferred by attaching them to the proxy VM. This approach does not require an ****export storage domain to be set up.

Please make sure to follow these steps: [LVM setup on vProtect Node for disk attachment backup mode](../../common-tasks/lvm-setup-on-vprotect-node-for-disk-attachment-backup-mode.md).

### Disk image transfer API

This API appeared in OLVM 4.2 and allowed the export of individual snapshots directly from the OLVM manager. So, instead of having to install multiple Proxy VMs, you can have a single external Node installation, which just invokes APIs via OLVM manager.

This strategy supports incremental backups. Assuming you have OLVM 4.2 or newer – just add your manager to vProtect and setup is done. From a network perspective - it requires two additional ports to open 54322 and 54323 and your data to be pulled from the hypervisor manager.

Unfortunately, there are a few problems with the current architecture of this solution. The biggest issue is that all traffic passes via OLVM manager, which may impact transfer rates that you can achieve during the backup process. To put that into perspective – in disk attachment, you can basically read data as if it is a local drive, where it could potentially be deduplicated even before transferring it to the backup destination.

![](../../../.gitbook/assets/deployment-vprotect-olvm-disk-image-transfer.png)

#### Backup Process

* crash-consistent snapshot using hypervisor's API
* optionally FS freeze can be executed before snapshot can be executed \(FS thaw once the snapshot is completed\) if enabled and guest tools installed inside
* optional application consistency using pre/post snapshot command execution
* supported for oVirt/RHV/OLVM 4.3+
* metadata exported from API
* data transfer initiated on the manager and actual data exported from the hypervisor using imageio API
* incremental backups use the same APIs, but requests for changed blocks only
* the last snapshot kept on the hypervisor for the next incremental backup \(if at least one schedule assigned to the VM has backup type set to incremental\)
* restore recreates VM from metadata using API and imports merged chain of data for each disk using imageio API

Disk image transfer mode exports data directly using OLVM 4.2+ API. There is no need to setup an export storage domain or LVM. This mode uses snapshot-chains provided by OLVM.

You may need to open communication for additional port **54323** on the OLVM manager - it needs to be accessible from vProtect Node. Also, make sure that your **ovirt-imageio-proxy** services are running and properly configured \(you can verify it by trying to upload images with OLVM UI\).

Follow the steps in this section: [Full versions of libvirt/qemu packages installation](../../common-tasks/full-versions-of-libvirt-qemu-packages-installation.md).

### SSH transfer

This is an enhancement for the disk image transfer API strategy. It allows vProtect to use OLVM API v4.2+ \(HTTPS connection to OLVM manager\) only to collect metadata. Backup is done over SSH directly from the hypervisor \(optionally using netcat for transfer\), import is also using SSH \(without netcat option\). No need to install a node on the OLVM environment. This method can significantly boost backup transfers and supports incremental backups.

![](../../../.gitbook/assets/deployment-vprotect-olvm-ssh-transfer.png)

#### Backup Process

* crash-consistent snapshot using hypervisor's API
* optionally FS freeze can be executed before snapshot can be executed \(FS thaw once the snapshot is completed\) if enabled and guest tools installed inside
* optional application consistency using pre/post snapshot command execution • metadata exported from API
* data transfer via SSH \(optional using netcat\) - the full chain of disk snapshot files for each disk o if LVM-based storage is used, then node activates volumes if necessary to read data o if Gluster FS is used, then disk files are copied directly
* incremental backup export just sub-chain of QCOW2-deltas snapshots since last stored snapshot
* the last snapshot kept on the hypervisor for the next incremental backup \(if at least one schedule assigned to the VM has backup type set to incremental\)
* restore recreates VM with empty storage from metadata using API and imports merged data over SSH to appropriate location on a hypervisor

This method assumes that all data transfers are directly from the hypervisor - over SSH. This means that after adding OLVM manager and detecting all available hypervisors - **you need to also provide SSH credentials or SSH keys for each of the hypervisors**. You can also use [SSH public key authentication](../../common-tasks/ssh-public-key-authentication.md).

