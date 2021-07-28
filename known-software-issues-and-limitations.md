# Known software issues and limitations

<table>
  <thead>
    <tr>
      <th style="text-align:left">Issue ID</th>
      <th style="text-align:left">Product feature</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Workaround</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">0001</td>
      <td style="text-align:left">Task cancellation</td>
      <td style="text-align:left">Task cancellation process will not reflect in immediate task failure -
        task state will be changed to cancelled and only when engine checks for
        it&apos;s state again it will initiate cancellation operation - some platforms
        may require even data transfer to complete before</td>
      <td style="text-align:left">Allow task to cancel and fail gracefully - this will allow vProtect to
        clean up temporary artifacts. If you click again task will be forced to
        be removed from the queue and artifacts such as snapshots will be removed
        as a part of daily snapshot cleanup job. In general avoid forced removal
        of tasks.</td>
    </tr>
    <tr>
      <td style="text-align:left">0002</td>
      <td style="text-align:left">Storage usage statistics</td>
      <td style="text-align:left">Storage statistics are updated after each backup or clean old backups
        job - this data may not be up-to-date all the time</td>
      <td style="text-align:left">To have current storage usage updated you can invoke Clean Old backups
        job from Backup Destinations tab</td>
    </tr>
    <tr>
      <td style="text-align:left">0003</td>
      <td style="text-align:left">Pre/post access storage command execution</td>
      <td style="text-align:left">Complete command cannot be provided as a single string. Commands need
        to have their arguments provided as separate entries (by clicking <b>Add command arg</b> button).
        Commands are directly executed using OS-level calls so shell operators
        are not supported directly.</td>
      <td style="text-align:left">To use shell-specific operators/commands etc., execute commands with 3
        command arguments <code>/bin/bash</code>, <code>-c</code>, <code>your command-with-all-arguments-and-shell-operators</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">0004</td>
      <td style="text-align:left">Tasks stuck in the queue in Queued state</td>
      <td style="text-align:left">Tasks will be usually be executed according to the limits set on the node
        and only if the node is running and has available space on the staging</td>
      <td
      style="text-align:left">Verify node has available space in the staging space path - there should
        be a warning message in <code>vprotect_daemon.log</code>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">0005</td>
      <td style="text-align:left">OpenStack backup using disk attachment</td>
      <td style="text-align:left">OpenStack with disk-attachment backup strategy (cinder) - 3.9.2 only supports
        Ceph RBD as a storage backend</td>
      <td style="text-align:left">N/A</td>
    </tr>
    <tr>
      <td style="text-align:left">0006</td>
      <td style="text-align:left">KVM stand-alone - disk formats</td>
      <td style="text-align:left">VMs being backed up must have virtual disks as QCOW2/RAW files or LVM
        volumes</td>
      <td style="text-align:left">N/A</td>
    </tr>
    <tr>
      <td style="text-align:left">0007</td>
      <td style="text-align:left">KVM stand-alone - snapshots</td>
      <td style="text-align:left">Snapshots on KVM hypervisors are made using libvirt (QCOW2/RAW files)
        or LVM snapshots and are created per-volume basis; this operation may not
        be atomic if multiple drives are used</td>
      <td style="text-align:left">Make sure data in the VM (especially file systems reside on as few disks
        as possible to lower the risks of the data inconsistency) or try to use
        pre-post remote command execution to quiesce application before a snapshot
        is done</td>
    </tr>
    <tr>
      <td style="text-align:left">0008</td>
      <td style="text-align:left">KVM standalone incremental backup on QCOW2</td>
      <td style="text-align:left">Incremental backups will be performed only on running VMs. libvirt doesn&apos;t
        allow block commit on a power-down VM so snapshot wouldn&apos;t be removed.</td>
      <td
      style="text-align:left">Full backup will be performed instead</td>
    </tr>
    <tr>
      <td style="text-align:left">0009</td>
      <td style="text-align:left">Backup providers path</td>
      <td style="text-align:left">Backup provider&apos;s paths must be mounted and available ahead - you
        should provide path just to the mount point, without any protocol specification</td>
      <td
      style="text-align:left">Mount remote file systems first and make sure these are available all
        the time - in vProtect configuration please provide just locally available
        mount point</td>
    </tr>
    <tr>
      <td style="text-align:left">0010</td>
      <td style="text-align:left">Backups marked as Success (removed)</td>
      <td style="text-align:left">When the backup completes or when clean old backups job is performed vProtect
        marks non-present backups as removed (if any of the files that were a part
        of the backup is not present). This may also happen if your storage was
        not available temporarily.</td>
      <td style="text-align:left">Make sure storage and all of the files are available and run the Clean
        Old Backups job - this job also attempts to sync files present in the backup
        provider again with the database - if all files for a particular backup
        are found again (and all previous backups that this particular backups
        depends on also are present) it will have the status of Success again</td>
    </tr>
    <tr>
      <td style="text-align:left">0011</td>
      <td style="text-align:left">Hypervisor storage usage statistics</td>
      <td style="text-align:left">Restore may fail due to insufficient storage space in Hypervisor Storage
        used as a target because of usage information that is not up to date. Usage
        statistics are updated only with inventory synchronization job</td>
      <td
      style="text-align:left">Run Inventory Synchronization job again on your hypervisor (or manager)
        to update storage statistics and try to restore again.</td>
    </tr>
    <tr>
      <td style="text-align:left">0012</td>
      <td style="text-align:left">RHV - SSH transfer rate drops after some time</td>
      <td style="text-align:left">SSH transfer rate may drop in some environments when used intensively
        over a longer time.</td>
      <td style="text-align:left">If possible and when the network used for transfers is trusted, please
        use the netcat option to transfer files outside of the SSH channel</td>
    </tr>
    <tr>
      <td style="text-align:left">0013</td>
      <td style="text-align:left">AWS EC2 - AMIs left in the account</td>
      <td style="text-align:left">For AWS EC2 some instances require the original base image to be restored
        - this is especially true for Windows-based clients where license relates
        to the original disk image. If an image is not left, vProtect can only
        restore such guests by creating a new one from the new image (as a root
        device) that is available and attach data volumes. AMIs are kept as long
        as the particular backup is going to use them and will be removed together.</td>
      <td
      style="text-align:left">For such guests, we recommend to enable Windows (or Linux) image required
        option in your Hypervisor Manager details</td>
    </tr>
    <tr>
      <td style="text-align:left">0014</td>
      <td style="text-align:left">AWS additional costs</td>
      <td style="text-align:left">Notice that vProtect needs sometimes to transfer EBS volumes between AZ
        if it resides in a different AZ then the node - AWS charges for intra-AZ
        transfers</td>
      <td style="text-align:left">Recommended deployment is in the same AZ as the VMs that node is going
        to protect to limit the number of transfers</td>
    </tr>
    <tr>
      <td style="text-align:left">0015</td>
      <td style="text-align:left">Node tasks limits</td>
      <td style="text-align:left">The number of concurrent tasks are configured in the Node Configuration
        -&gt; Tasks section. These limits apply to all nodes that use this particular
        configuration. Currently, there is no global setting to limit the number
        of tasks for all of the nodes in the environment.</td>
      <td style="text-align:left">To limit the number of tasks globally, reduce numbers in individual node
        configurations.</td>
    </tr>
    <tr>
      <td style="text-align:left">0016</td>
      <td style="text-align:left">Hypervisor-specific settings in Node Configuration</td>
      <td style="text-align:left">All of the configuration parameters in the Hypervisors tab in Node Configuration
        are applied to all nodes with this configuration - regardless of which
        hypervisor it is attached to. This implies that Proxmox settings such as
        compression will have to be the same on all hypervisors handled by nodes
        having the same configuration assigned will have to be the same on all
        of these hypervisors.</td>
      <td style="text-align:left">To use these settings with different values for some hypervisors you need
        to assign separate nodes and define separate node configurations. Finally,
        assign separate nodes for these hypervisors.</td>
    </tr>
    <tr>
      <td style="text-align:left">0017</td>
      <td style="text-align:left">Inventory synchronization - duplicated UUID</td>
      <td style="text-align:left">In some cases, it may happen that the same storage was previously detected
        with a different setup and remained in the database.</td>
      <td style="text-align:left">Remove unused hypervisor storage and try to invoke inventory synchronization
        again.</td>
    </tr>
    <tr>
      <td style="text-align:left">0018</td>
      <td style="text-align:left">Estimated backup size of policy</td>
      <td style="text-align:left">The estimated backup size of a policy is computed based only on a known
        backup sizes and extrapolated to the rest of the VMs in the group. This
        implies that estimation will use the average backup size and multiply it
        by the number of all VMs in the group. Even though disk sizes are known
        it is not always the same as the size of the backups (especially considering
        compression or the fact that some strategies require chains of backup deltas
        to be exported)</td>
      <td style="text-align:left">Wait a longer period of time, once more and more backups are completed
        this estimation will be closer to real value.</td>
    </tr>
    <tr>
      <td style="text-align:left">0019</td>
      <td style="text-align:left">Citrix Hypervisor/ xcp-ng - transfers</td>
      <td style="text-align:left">Transfer NIC is not used in incremental backups when CBT strategy is invoked
        - Citrix/xcp-ng may require NBD to be exposed by the master - so vProtect
        has to read from the address provided by the CBT mechanism in order to
        connect to the NBD device. Also in some cases, data can only be transferred
        from the master host (especially when it is powered down)</td>
      <td style="text-align:left">Allow network traffic between all hypervisors and corresponding nodes
        in the same pool, as sometimes actual transfer may occur from the master
        host instead of the one which hosts VM.</td>
    </tr>
    <tr>
      <td style="text-align:left">0020</td>
      <td style="text-align:left">RHV - SSH Transfer permissions on the hypervisor</td>
      <td style="text-align:left">SSH Transfer for RHV requires usually root permissions on the hypervisor
        in order to activate/deactivate LVM volumes for the backup</td>
      <td style="text-align:left">You only may try with a different backup strategy such as Disk-attachment
        or Disk Image Transfer</td>
    </tr>
    <tr>
      <td style="text-align:left">0021</td>
      <td style="text-align:left">RHV - SSH Transfer - hypervisor access</td>
      <td style="text-align:left">vProtect using SSH Transfer for RHV environments needs to be able to access
        all hypervisors in the cluster - as it may happen that the created disk
        is available only on a subset of them and needs to be transferred or recovered
        only by using this specific hypervisor</td>
      <td style="text-align:left">Allow network traffic and provide valid credentials to access all hypervisors
        in the cluster over SSH.</td>
    </tr>
    <tr>
      <td style="text-align:left">0022</td>
      <td style="text-align:left">Nutanix VG support</td>
      <td style="text-align:left">vProtect 3.9.2 only supports volumes residing on the storage containers
        - VGs are not supported yet</td>
      <td style="text-align:left">N/A</td>
    </tr>
    <tr>
      <td style="text-align:left">0023</td>
      <td style="text-align:left">Nutanix Prism Element/Central connectivity</td>
      <td style="text-align:left">vProtect is able to perform backups by using APIs provided by Prism Element
        only - Prism Central doesn&apos;t offer backup API</td>
      <td style="text-align:left">Connect to your Prism elements by specifying separate Hypervisor Managers
        in vProtect (not by pointing to the Prism Central)</td>
    </tr>
    <tr>
      <td style="text-align:left">0024</td>
      <td style="text-align:left">Nutanix backup consistency for intensively used VMs</td>
      <td style="text-align:left">Intensive workload on the VM may affect backup consistency when using
        crash-consistent backup</td>
      <td style="text-align:left">If you need higher consistency install Nutanix Guest Tools inside your
        VM and enable application-consistent snapshots in the VM details in vProtect</td>
    </tr>
    <tr>
      <td style="text-align:left">0025</td>
      <td style="text-align:left">iSCSI shares for RAW backups</td>
      <td style="text-align:left">RAW backups allow vProtect to share them over iSCSI. If backups are in
        other formats (such as QCOW2) these cannot currently be shared over iSCSI</td>
      <td
      style="text-align:left">Use automatic mount instead or restore a backup to the file system and
        mount them using external tools such as qemu-nbd for QCOW2 files</td>
    </tr>
    <tr>
      <td style="text-align:left">0026</td>
      <td style="text-align:left">Backup and snapshot policies assignment</td>
      <td style="text-align:left">Only 1 backup and 1 snapshot management policy can be assigned to the
        given VM</td>
      <td style="text-align:left">If you need a dedicated setting for a single VM, you need to create a
        separate policy for that VM</td>
    </tr>
    <tr>
      <td style="text-align:left">0027</td>
      <td style="text-align:left">1 schedule per rule</td>
      <td style="text-align:left">Currently, each schedule can only be assigned to the single rule within
        the same policy.</td>
      <td style="text-align:left">If you need to execute two rules at the same time, you need to create
        separate schedules and assign to these rules</td>
    </tr>
    <tr>
      <td style="text-align:left">0028</td>
      <td style="text-align:left">Staging space</td>
      <td style="text-align:left">Staging space is an integral part of a node - it allows us to mix backup
        strategies, especially based on the export storage domain/repository approach
        with other methods and file system scanning for future file-level restores.
        It needs to be available at all times and <code>vprotect</code> user needs
        to be able to write to all subdirectories.</td>
      <td style="text-align:left">To save space and boost backup time (direct writes to the backup destination)
        you can however mount staging and your backup destination in the same directory
        - <code>/vprotect_data</code>. Please remember to point the backup destination&apos;s
        path to a subdirectory of this mount point, such as <code>/vprotect_data/backups</code> -
        still on the same FS, but paths must be different.</td>
    </tr>
    <tr>
      <td style="text-align:left">0029</td>
      <td style="text-align:left">Node OS-level permissions</td>
      <td style="text-align:left">On the OS level vProtect requires significant permissions to be able to
        manipulate disks, scan for file systems, mount them, expose resources over
        iSCSI, operate on block devices (NBD/iSCSI/RBD) and more. These unfortunately
        require multiple sudo entries and SELinux is disabled at the moment.</td>
      <td
      style="text-align:left">
        <p>If some features are not required at the moment - including NBD/iSCSI/NFS
          related - you may reduce the number of entries in <code>/etc/sudoers.d/01-vprotect_node</code>
        </p>
        <p>You also can try to enable SELinux, but later you need to track SELinux
          errors and add appropriate permissions when some of the functionality is
          blocked</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">0030</td>
      <td style="text-align:left">Proxmox VE</td>
      <td style="text-align:left">CBT backup strategy</td>
      <td style="text-align:left">Qcow2 virtual machines are required to use the new Proxmox VE backup strategy
        - Change block tracking ( CBT )</td>
    </tr>
    <tr>
      <td style="text-align:left">0031</td>
      <td style="text-align:left">Proxmox VE</td>
      <td style="text-align:left">CBT backup strategy</td>
      <td style="text-align:left">At the moment we do not support the &quot;Dirty Bitmaps&quot; function,
        therefore we require the last snapshot to be left for incremental backups.</td>
    </tr>
    <tr>
      <td style="text-align:left">0032</td>
      <td style="text-align:left">Storage Providers and node assignment</td>
      <td style="text-align:left">vProtect supports only one node assigned to Storage Provider, which means
        that backup of significantly big volumes from bigger storage providers,
        i.e. Ceph RBD etc. will require high performing node and cannot be scaled
        out by adding nodes</td>
      <td style="text-align:left">Install multiple vProtect Server+Node environments and protect the non-overlapping
        set of volumes with each vProtect instance.</td>
    </tr>
    <tr>
      <td style="text-align:left">0033</td>
      <td style="text-align:left">RHV - Restore with SPARSE disk allocation format</td>
      <td style="text-align:left">Restore to RHV using SPARSE disk allocation format is not supported
        if backup files are in RAW format and destination storage domain type in either Fibre Channel
        or iSCSI. If such configuration is detected, then disk allocation format is automatically
        switched to PREALLOCATED</td>
      <td style="text-align:left">You can use other backup strategies that use QCOW2 files
        instead of RAW (like disk image transfer). Alternatively, you can select different storage domain
        of a type that supports SPARSE disks with RAW files</td>
    </tr>
  </tbody>
</table>

