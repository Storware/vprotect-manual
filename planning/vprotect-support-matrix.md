# vProtect Support Matrix

## Backup Destination Support

### Filesystem

| Backup Provider | Supported version | Random Access | Deduplication | Encryption | Pre/post access command execution |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Filesystem | N/A | ✅ | ✅ \(built-in VDO\) | ✅ | ✅ |
| Filesystem \(synthetic, XFS\) | Linux kernel 4.15+, xfsprogs 4.17+ | ✅ | ✅ \(built-in VDO\) | ❌ | ✅ |
| PowerProtect DD | DD OS 7.x | ✅ | ✅ | ✅ | ✅ |

### Object storage

| Backup Provider | Supported version | Random Access | Deduplication | Encryption | Pre/post access command execution |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Amazon S3 | Current | ❌ | N/A | ✅ | ✅ |
| S3 compatible | Current | ❌ | N/A | ✅ | ✅ |
| MS Azure Blob Storage | Current | ❌ | N/A | ✅ | ✅ |
| OpenStack Swift | API v2 | ❌ | N/A | ❌ | ✅ |
| Scality Ring | 6.4+ | ❌ | N/A | Provider dependent | ✅ |

### Enterprise backup providers

| Backup Provider | Supported version | Random Access | Deduplication | Encryption | Pre/post access command execution |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Dell EMC Avamar | 7.5+ | ❌ | ✅ | Provider dependent | ✅ |
| Dell EMC Networker | 9+ | ❌ | ✅ | Provider dependent | ✅ |
| IBM Spectrum Protect | 7.1.5+ | ❌ | ✅ | Provider dependent | ✅ |
| Veritas Netbackup | 7.5+ | ❌ | ✅ | Provider dependent | ✅ |

## Virtualization Platforms - features and requirements

## Virtual Machines

### VMware vSphere/ESXi

|  | NBD & HotAdd |
| :--- | :--- |
| Minimum version | 6.x+ |
| Status | In operation \(**preferred**\) |
| Last snapshot kept on hypervisor for inc. backups | yes |
| Hypervisor OS access needed | No |
| Proxy VM needed | no \(optional for HotAdd backup\) |

| Feature | NBD & HotAdd |
| :--- | :--- |
| Incremental backup | ✅ |
| File-level restore | ✅ |
| VM disk exclusion | ✅ |
| Snapshot management | ✅ |
| Quiesced snapshot | ❌ \(hypervisor-dependent\) |
| Pre/post snapshot command execution | ✅ |
| Backup disks sharable over iSCSI | ✅ |
| Name-based policy assignment | ✅ |
| Tag-based policy assignment | ✅ |
| Available space for snapshot check | ❌ \(hypervisor-dependent\) |
| Power-on VM after restore | ✅ |

### Microsoft Hyper-V

|  | 2016 & 2019 |
| :--- | :--- |
| Minimum version | Windows Server 2016+ |
| Status | In operation \(**preferred**\) |
| Last snapshot kept on hypervisor for inc. backups | yes |
| Hypervisor OS access needed | No |
| Proxy VM needed | no |

| Feature | 2016 & 2019 |
| :--- | :--- |
| Incremental backup | ✅ |
| File-level restore | ✅ |
| VM disk exclusion | ✅ |
| Snapshot management | ✅ |
| Quiesced snapshot | ❌ \(hypervisor-dependent\) |
| Pre/post snapshot command execution | ✅ |
| Backup disks sharable over iSCSI | ✅ |
| Name-based policy assignment | ✅ |
| Tag-based policy assignment | ❌ |
| Available space for snapshot check | ❌ \(hypervisor-dependent\) |
| Power-on VM after restore | ✅ |

### Nutanix AHV

|  | Disk-attachment |
| :--- | :--- |
| Minimum version | 5.5+ \(Intel-only\) |
| Status | In operation \(**preferred**\) |
| Last snapshot kept on hypervisor for inc. backups | yes |
| Hypervisor OS access needed | no |
| Proxy VM needed | yes |

| Feature | Disk-attachment |
| :--- | :--- |
| Incremental backup | ✅ |
| File-level restore | ✅ |
| VM disk exclusion | ✅ |
| Snapshot management | ✅ |
| Quiesced snapshot | ✅ |
| Pre/post snapshot command execution | ✅ |
| Backup disks sharable over iSCSI | ✅ |
| Name-based policy assignment | ✅ |
| Tag-based policy assignment | ✅ \(when using Prism Central\) |
| Available space for snapshot check | ❌ \(hypervisor-dependent\) |
| Power-on VM after restore | ✅ |

### Red Hat Virtualization

<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">Export storage domain</th>
      <th style="text-align:left">Disk-attachment</th>
      <th style="text-align:left">Disk Image Transfer</th>
      <th style="text-align:left">SSH Transfer</th>
      <th style="text-align:left">Changed-Block Tracking</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Minimum version</td>
      <td style="text-align:left">3.5.1+</td>
      <td style="text-align:left">4.0+</td>
      <td style="text-align:left">4.3+</td>
      <td style="text-align:left">4.3+</td>
      <td style="text-align:left">4.4+</td>
    </tr>
    <tr>
      <td style="text-align:left">Status</td>
      <td style="text-align:left">To be deprecated</td>
      <td style="text-align:left">In operation</td>
      <td style="text-align:left">In operation</td>
      <td style="text-align:left">In operation (<b>preferred</b>)</td>
      <td style="text-align:left">Technical preview</td>
    </tr>
    <tr>
      <td style="text-align:left">Last snapshot kept on hypervisor for inc. backups</td>
      <td style="text-align:left">yes</td>
      <td style="text-align:left">yes</td>
      <td style="text-align:left">yes</td>
      <td style="text-align:left">yes</td>
      <td style="text-align:left">no</td>
    </tr>
    <tr>
      <td style="text-align:left">Hypervisor OS access needed</td>
      <td style="text-align:left">no</td>
      <td style="text-align:left">no</td>
      <td style="text-align:left">no</td>
      <td style="text-align:left">yes</td>
      <td style="text-align:left">no</td>
    </tr>
    <tr>
      <td style="text-align:left">Proxy VM needed</td>
      <td style="text-align:left">no</td>
      <td style="text-align:left">yes</td>
      <td style="text-align:left">no</td>
      <td style="text-align:left">no</td>
      <td style="text-align:left">no</td>
    </tr>
    <tr>
      <td style="text-align:left">Key caveats</td>
      <td style="text-align:left">
        <p>using old API v3</p>
        <p>additional snapshot-cloning required</p>
      </td>
      <td style="text-align:left">
        <p>full backup only</p>
        <p>disk attachment process may be slow</p>
      </td>
      <td style="text-align:left">data transfer via Manager</td>
      <td style="text-align:left">access to the hypervisor needed</td>
      <td style="text-align:left">using technical preview APIs</td>
    </tr>
  </tbody>
</table>

| Feature | Export storage domain | Disk-attachment | Disk Image Transfer | SSH Transfer | Changed-Block Tracking |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Incremental backup | ❌ | ❌ | ✅ | ✅ | ✅ |
| File-level restore | ✅ | ✅ | ✅ | ✅ | ✅ |
| VM disk exclusion | ❌ | ✅ | ✅ | ✅ | ✅ |
| Snapshot management | ✅ | ✅ | ✅ | ✅ | ✅ |
| Quiesced snapshot | ✅ | ✅ | ✅ | ✅ | ✅ |
| Pre/post snapshot command execution | ✅ | ✅ | ✅ | ✅ | ✅ |
| Backup disks sharable over iSCSI | ✅ | ✅ | ✅ \(RAW-only\) | ✅ \(RAW-only\) | ✅ |
| Name-based policy assignment | ✅ | ✅ | ✅ | ✅ | ✅ |
| Tag-based policy assignment | ✅ | ✅ | ✅ | ✅ | ✅ |
| Available space for snapshot check | ✅ | ✅ | ✅ | ✅ | ✅ |
| Power-on VM after restore | ❌ | ✅ | ✅ | ✅ | ✅ |

### oVirt

<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">Export storage domain</th>
      <th style="text-align:left">Disk-attachment</th>
      <th style="text-align:left">Disk Image Transfer</th>
      <th style="text-align:left">SSH Transfer</th>
      <th style="text-align:left">Changed-Block Tracking</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Minimum version</td>
      <td style="text-align:left">3.5.1+</td>
      <td style="text-align:left">4.0+</td>
      <td style="text-align:left">4.3+</td>
      <td style="text-align:left">4.3+</td>
      <td style="text-align:left">4.4+</td>
    </tr>
    <tr>
      <td style="text-align:left">Status</td>
      <td style="text-align:left">To be deprecated</td>
      <td style="text-align:left">In operation</td>
      <td style="text-align:left">In operation</td>
      <td style="text-align:left">In operation (<b>preferred</b>)</td>
      <td style="text-align:left">Technical preview</td>
    </tr>
    <tr>
      <td style="text-align:left">Last snapshot kept on hypervisor for inc. backups</td>
      <td style="text-align:left">yes</td>
      <td style="text-align:left">yes</td>
      <td style="text-align:left">yes</td>
      <td style="text-align:left">yes</td>
      <td style="text-align:left">no</td>
    </tr>
    <tr>
      <td style="text-align:left">Hypervisor OS access needed</td>
      <td style="text-align:left">no</td>
      <td style="text-align:left">no</td>
      <td style="text-align:left">no</td>
      <td style="text-align:left">yes</td>
      <td style="text-align:left">no</td>
    </tr>
    <tr>
      <td style="text-align:left">Proxy VM needed</td>
      <td style="text-align:left">no</td>
      <td style="text-align:left">yes</td>
      <td style="text-align:left">no</td>
      <td style="text-align:left">no</td>
      <td style="text-align:left">no</td>
    </tr>
    <tr>
      <td style="text-align:left">Key Caveats</td>
      <td style="text-align:left">
        <p>using old API v3</p>
        <p>additional snapshot-cloning required</p>
      </td>
      <td style="text-align:left">
        <p>full backup only</p>
        <p>disk attachment process may be slow</p>
      </td>
      <td style="text-align:left">data transfer via Manager</td>
      <td style="text-align:left">access to the hypervisor needed</td>
      <td style="text-align:left">using technical preview APIs</td>
    </tr>
  </tbody>
</table>

| Feature | Export storage domain | Disk-attachment | Disk Image Transfer | SSH Transfer | Changed-Block Tracking |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Incremental backup | ❌ | ❌ | ✅ | ✅ | ✅ |
| File-level restore | ✅ | ✅ | ✅ | ✅ | ✅ |
| VM disk exclusion | ❌ | ✅ | ✅ | ✅ | ✅ |
| Snapshot management | ✅ | ✅ | ✅ | ✅ | ✅ |
| Quiesced snapshot | ✅ | ✅ | ✅ | ✅ | ✅ |
| Pre/post snapshot command execution | ✅ | ✅ | ✅ | ✅ | ✅ |
| Backup disks sharable over iSCSI | ✅ | ✅ | ✅ \(RAW-only\) | ✅ \(RAW-only\) | ✅ |
| Name-based policy assignment | ✅ | ✅ | ✅ | ✅ | ✅ |
| Tag-based policy assignment | ✅ | ✅ | ✅ | ✅ | ✅ |
| Available space for snapshot check | ✅ | ✅ | ✅ | ✅ | ✅ |
| Power-on VM after restore | ❌ | ✅ | ✅ | ✅ | ✅ |

### Oracle Linux Virtualization Manager

<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">Disk-attachment</th>
      <th style="text-align:left">Disk Image Transfer</th>
      <th style="text-align:left">SSH Transfer</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Minimum version</td>
      <td style="text-align:left">4.3+</td>
      <td style="text-align:left">4.3+</td>
      <td style="text-align:left">4.3+</td>
    </tr>
    <tr>
      <td style="text-align:left">Status</td>
      <td style="text-align:left">In operation</td>
      <td style="text-align:left">In operation</td>
      <td style="text-align:left">In operation (<b>preferred</b>)</td>
    </tr>
    <tr>
      <td style="text-align:left">Last snapshot kept on hypervisor for inc. backups</td>
      <td style="text-align:left">yes</td>
      <td style="text-align:left">yes</td>
      <td style="text-align:left">yes</td>
    </tr>
    <tr>
      <td style="text-align:left">Hypervisor OS access needed</td>
      <td style="text-align:left">no</td>
      <td style="text-align:left">no</td>
      <td style="text-align:left">yes</td>
    </tr>
    <tr>
      <td style="text-align:left">Proxy VM needed</td>
      <td style="text-align:left">yes</td>
      <td style="text-align:left">no</td>
      <td style="text-align:left">no</td>
    </tr>
    <tr>
      <td style="text-align:left">Key Caveats</td>
      <td style="text-align:left">
        <p>full backup only</p>
        <p>disk attachment process may be slow</p>
      </td>
      <td style="text-align:left">data transfer via Manager</td>
      <td style="text-align:left">access to he hypervisor needed</td>
    </tr>
  </tbody>
</table>

| Feature | Disk-attachment | Disk Image Transfer | SSH Transfer |
| :--- | :--- | :--- | :--- |
| Incremental backup | ❌ | ✅ | ✅ |
| File-level restore | ✅ | ✅ | ✅ |
| VM disk exclusion | ✅ | ✅ | ✅ |
| Snapshot management | ✅ | ✅ | ✅ |
| Quiesced snapshot | ✅ | ✅ | ✅ |
| Pre/post snapshot command execution | ✅ | ✅ | ✅ |
| Backup disks sharable over iSCSI | ✅ | ✅ \(RAW-only\) | ✅ \(RAW-only\) |
| Name-based policy assignment | ✅ | ✅ | ✅ |
| Tag-based policy assignment | ✅ | ✅ | ✅ |
| Available space for snapshot check | ✅ | ✅ | ✅ |
| Power-on VM after restore | ✅ | ✅ | ✅ |

### Proxmox VE

<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">Export storage repository</th>
      <th style="text-align:left">SSH Transfer</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Minimum version</td>
      <td style="text-align:left">5.2+</td>
      <td style="text-align:left">5.2+</td>
    </tr>
    <tr>
      <td style="text-align:left">Status</td>
      <td style="text-align:left">In operation</td>
      <td style="text-align:left">In operation (<b>preferred</b>)</td>
    </tr>
    <tr>
      <td style="text-align:left">Last snapshot kept on hypervisor for inc. backups</td>
      <td style="text-align:left">yes</td>
      <td style="text-align:left">yes</td>
    </tr>
    <tr>
      <td style="text-align:left">Hypervisor OS access needed</td>
      <td style="text-align:left">yes</td>
      <td style="text-align:left">yes</td>
    </tr>
    <tr>
      <td style="text-align:left">Proxy VM needed</td>
      <td style="text-align:left">no</td>
      <td style="text-align:left">no</td>
    </tr>
    <tr>
      <td style="text-align:left">Key Caveats</td>
      <td style="text-align:left">
        <p>using single image export</p>
        <p>file-level restore requires additional image extraction</p>
      </td>
      <td style="text-align:left">only QCOW2 based disks supported</td>
    </tr>
  </tbody>
</table>

| Feature | Export storage repository | SSH Transfer |
| :--- | :--- | :--- |
| Incremental backup | ❌ | ✅ |
| File-level restore | ✅ | ✅ |
| VM disk exclusion | ✅ | ✅ |
| Snapshot management | ✅ | ✅ |
| Quiesced snapshot | ✅ | ✅ |
| Pre/post snapshot command execution | ✅ | ✅ |
| Backup disks sharable over iSCSI | ❌ | ✅ |
| Name-based policy assignment | ✅ | ✅ |
| Tag-based policy assignment | ❌ | ❌ |
| Available space for snapshot check | ❌ \(hypervisor-dependent\) | ✅ |
| Power-on VM after restore | ✅ | ✅ |

### KVM/Xen

|  | SSH Transfer |
| :--- | :--- |
| Minimum version | QEMU 2.1+ \(qcow2-based VMs need libvirt with block commit feature\), libvirt |
| Status | In operation \(**preferred**\) |
| Last snapshot kept on hypervisor for inc. backups | yes |
| Hypervisor OS access needed | yes |
| Proxy VM needed | no |
| VM storage formats | QCOW2/RAW file, Ceph RBD volume, LVM volume, LVM-thin volume |

| Feature | SSH Transfer |
| :--- | :--- |
| Incremental backup | ✅ |
| File-level restore | ✅ |
| VM disk exclusion | ✅ |
| Snapshot management | ❌ |
| Quiesced snapshot | ❌ \(hypervisor-dependent\) |
| Pre/post snapshot command execution | ✅ |
| Backup disks sharable over iSCSI | ✅ |
| Name-based policy assignment | ✅ |
| Tag-based policy assignment | ❌ |
| Available space for snapshot check | ❌ \(hypervisor-dependent\) |
| Power-on VM after restore | ✅ |

### OpenStack

|  | Disk-attachment | SSH Transfer |
| :--- | :--- | :--- |
| Minimum version | Queens | Queens, QEMU 2.1+ \(qcow2-based VMs need libvirt with block commit feature\) |
| Status | In operation \(**preferred**\) | In operation |
| Last snapshot kept on hypervisor for inc. backups | yes | yes |
| Hypervisor OS access needed | no | yes |
| Proxy VM needed | yes | no |
| Key Caveats | incremental backups only for Ceph RBD-based VMs | only QCOW2 based disks supported |

| Feature | Disk-attachment | SSH Transfer |
| :--- | :--- | :--- |
| Incremental backup | ✅ \(Ceph RBD volumes only\) | ✅ |
| File-level restore | ✅ | ✅ |
| VM disk exclusion | ✅ | ✅ |
| Snapshot management | ❌ | ❌ |
| Quiesced snapshot | ❌ \(hypervisor-dependent\) | ❌ \(hypervisor-dependent\) |
| Pre/post snapshot command execution | ✅ | ✅ |
| Backup disks sharable over iSCSI | ✅ | ✅ \(RAW file/LVM only\) |
| Name-based policy assignment | ✅ | ✅ |
| Tag-based policy assignment | ❌ | ❌ |
| Available space for snapshot check | ❌ \(hypervisor-dependent\) | ❌ \(hypervisor-dependent\) |
| Power-on VM after restore | ❌ \(always on\) | ✅ |

### Oracle VM

|  | Export storage repository |
| :--- | :--- |
| Minimum version | 3.4+ |
| Status | In operation \(**preferred**\) |
| Last snapshot kept on hypervisor for inc. backups | N/A |
| Hypervisor OS access needed | no |
| Proxy VM needed | no |

| Feature | Export storage repository |
| :--- | :--- |
| Incremental backup | ❌ |
| File-level restore | ✅ |
| VM disk exclusion | ✅ |
| Snapshot management | ❌ |
| Quiesced snapshot | ❌ |
| Pre/post snapshot command execution | ❌ |
| Backup disks sharable over iSCSI | ✅ |
| Name-based policy assignment | ✅ |
| Tag-based policy assignment | ❌ |
| Available space for snapshot check | ❌ \(hypervisor-dependent\) |
| Power-on VM after restore | ❌ |

### Citrix Hypervisor \(XenServer\)

|  | Single image \(XVA\) | Changed-block Tracking |
| :--- | :--- | :--- |
| Minimum version | 6.5+ | 6.5+ \(incremental backup: 7.3+\) |
| Status | In operation | In operation \(**preferred**\) |
| Last snapshot kept on hypervisor for inc. backups | yes | yes |
| Hypervisor OS access needed | no | no |
| Proxy VM needed | no | no |
| Key Caveats | file-level restore not supported | incremental backups may require Citrix XenServer license |

| Feature | Single image \(XVA\) | Changed-block Tracking |
| :--- | :--- | :--- |
| Incremental backup | ✅ | ✅ |
| File-level restore | ❌ | ✅ |
| VM disk exclusion | ❌ | ✅ |
| Snapshot management | ✅ | ✅ |
| Quiesced snapshot | ✅ | ✅ |
| Pre/post snapshot command execution | ✅ | ✅ |
| Backup disks sharable over iSCSI | ❌ | ✅ |
| Name-based policy assignment | ✅ | ✅ |
| Tag-based policy assignment | ✅ | ✅ |
| Available space for snapshot check | ✅ | ✅ |
| Power-on VM after restore | ✅ | ✅ |

### XCP-ng

|  | Single image \(XVA\) | Changed-block Tracking |
| :--- | :--- | :--- |
| Minimum version | 7.4+ | 7.4+ |
| Status | In operation | In operation \(**preferred**\) |
| Last snapshot kept on hypervisor for inc. backups | yes | yes |
| Hypervisor OS access needed | no | no |
| Proxy VM needed | no | no |
| Key Caveats | file-level restore not supported |  |

| Feature | Single image \(XVA\) | Changed-block Tracking |
| :--- | :--- | :--- |
| Incremental backup | ✅ | ✅ |
| File-level restore | ❌ | ✅ |
| VM disk exclusion | ❌ | ✅ |
| Snapshot management | ✅ | ✅ |
| Quiesced snapshot | ✅ | ✅ |
| Pre/post snapshot command execution | ✅ | ✅ |
| Backup disks sharable over iSCSI | ❌ | ✅ |
| Name-based policy assignment | ✅ | ✅ |
| Tag-based policy assignment | ✅ | ✅ |
| Available space for snapshot check | ✅ | ✅ |
| Power-on VM after restore | ✅ | ✅ |

### Huawei FusionCompute

|  | Changed Block Tracking |
| :--- | :--- |
| Minimum version | 8.0 |
| Status | In operation |
| Last snapshot kept on hypervisor for inc. backups | yes |
| Hypervisor OS access needed | no |
| Proxy VM needed | no |

| Feature | Changed Block Tracking |
| :--- | :--- |
| Incremental backup | ✅ |
| File-level restore | ✅ |
| VM disk exclusion | ✅ |
| Snapshot management | ✅ |
| Quiesced snapshot | ❌ |
| Pre/post snapshot command execution | ✅ |
| Backup disks sharable over iSCSI | ✅ |
| Name-based policy assignment | ✅ |
| Tag-based policy assignment | ❌ |
| Available space for snapshot check | ❌ |
| Power-on VM after restore | ❌ |

## Containers - features and requirements

### Kubernetes

|  | Helper pod/Ceph RBD |
| :--- | :--- |
| Minimum version | 1.10+ |
| Status | In operation \(**preferred**\) |
| Last snapshot kept on hypervisor for inc. backups | Yes |
| Hypervisor OS access needed | no |
| Proxy VM needed | no |

| Feature | Helper pod/Ceph RBD |
| :--- | :--- |
| Incremental backup | ✅ \(when using Ceph RBD as PV\) |
| File-level restore | ✅ \(when using Ceph RBD as PV\) |
| VM disk exclusion | ✅ |
| Snapshot management | ❌ |
| Quiesced snapshot | ✅ \(optional deployment pause\) |
| Pre/post snapshot command execution | ✅ \(post export\) |
| Backup disks sharable over iSCSI | ✅ \(when using Ceph RBD as PV\) |
| Name-based policy assignment | ✅ |
| Tag-based policy assignment | ✅ |
| Available space for snapshot check | N/A |
| Power-on VM after restore | ✅ |

### Red Hat OpenShift

|  | Helper pod/Ceph RBD |
| :--- | :--- |
| Minimum version | 4+ |
| Status | In operation \(**preferred**\) |
| Last snapshot kept on hypervisor for inc. backups | Yes |
| Hypervisor OS access needed | no |
| Proxy VM needed | no |

| Feature | Helper pod/Ceph RBD |
| :--- | :--- |
| Incremental backup | ✅ \(when using Ceph RBD as PV\) |
| File-level restore | ✅ \(when using Ceph RBD as PV\) |
| VM disk exclusion | ✅ |
| Snapshot management | ❌ |
| Quiesced snapshot | ✅ \(optional deployment pause\) |
| Pre/post snapshot command execution | ✅ \(post export\) |
| Backup disks sharable over iSCSI | ✅ \(when using Ceph RBD as PV\) |
| Name-based policy assignment | ✅ |
| Tag-based policy assignment | ✅ |
| Available space for snapshot check | N/A |
| Power-on VM after restore | ✅ |

### Proxmox VE

|  | Export storage repository |
| :--- | :--- |
| Minimum version | 5+ |
| Status | In operation \(**preferred**\) |
| Last snapshot kept on hypervisor for inc. backups | No |
| Hypervisor OS access needed | yes |
| Proxy VM needed | no |

| Feature | Export storage repository |
| :--- | :--- |
| Incremental backup | ❌ |
| File-level restore | ❌ |
| VM disk exclusion | ✅ |
| Snapshot management | ✅ |
| Quiesced snapshot | ❌ |
| Pre/post snapshot command execution | ✅ |
| Backup disks sharable over iSCSI | ❌ |
| Name-based policy assignment | ✅ |
| Tag-based policy assignment | ❌ |
| Available space for snapshot check | ❌ \(hypervisor-dependent\) |
| Power-on VM after restore | ✅ |

## Cloud - features and requirements

### AWS EC2

|  | Disk attachment |
| :--- | :--- |
| Minimum version | N/A |
| Status | In operation \(**preferred**\) |
| Last snapshot kept on hypervisor for inc. backups | yes |
| Hypervisor OS access needed | no |
| Proxy VM needed | yes |

| Feature | Disk attachment |
| :--- | :--- |
| Incremental backup | ❌ |
| File-level restore | ✅ |
| VM disk exclusion | ✅ |
| Snapshot management | ✅ |
| Quiesced snapshot | ❌ |
| Pre/post snapshot command execution | ✅ |
| Backup disks sharable over iSCSI | ✅ |
| Name-based policy assignment | ✅ |
| Tag-based policy assignment | ✅ |
| Available space for snapshot check | ❌ |
| Power-on VM after restore | ✅ |

## Storage Providers - features and requirements

### File system

|  | Mounted file system |
| :--- | :--- |
| Minimum version | N/A |
| Status | In operation \(**preferred**\) |
| Last snapshot kept in provider for inc. backups | N/A |
| Source type | any POSIX-compliant file system mounted on the node |

| Feature | Mounted file system |
| :--- | :--- |
| Incremental backup | ✅ |
| Incremental backup change source | file scan \(file modification time/size\) |
| File-level restore | ✅ |
| Snapshot management | ❌ |
| Pre/post snapshot command execution | ✅ |
| Backups sharable over iSCSI | ✅ |
| Name-based policy assignment | ❌ |

### Nutanix Files \(AFS\)

|  | File shares with CFT |
| :--- | :--- |
| Minimum version | N/A |
| Status | In operation \(**preferred**\) |
| Last snapshot kept in provider for inc. backups | N/A |
| Source type | NFS and SMB shares |

| Feature | File shares with CFT |
| :--- | :--- |
| Incremental backup | ✅ |
| Incremental backup change source | CFT API |
| File-level restore | ✅ |
| Snapshot management | ❌ |
| Pre/post snapshot command execution | ✅ |
| Backups sharable over iSCSI | ✅ |
| Name-based policy assignment | ✅ |

### Ceph RBD

|  | RBD export/RBD-NBD |
| :--- | :--- |
| Minimum version | Red Hat Ceph Storage 4.0+, Ceph nautilus+ |
| Status | In operation \(**preferred**\) |
| Last snapshot kept in provider for inc. backups | Yes |
| Source type | RBD volume |

| Feature | Base |
| :--- | :--- |
| Incremental backup | ✅ |
| Incremental backup change source | RBD snap-diff |
| File-level restore | ✅ |
| Snapshot management | ✅ |
| Pre/post snapshot command execution | ✅ |
| Backups sharable over iSCSI | ✅ |
| Name-based policy assignment | ✅ |

## Conditions / Exclusion

**File-level-restore**:  
_When backing up CentOS 8 with a disk with the GPT partition scheme, you can use file-level restore only on the vProtect node, which also runs on CentOS 8 operating system._