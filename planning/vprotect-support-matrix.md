# vProtect Support Matrix

## Backup Destination Support

### Filesystem

| Backup Provider | Supported version | Random Access | Deduplication | Encryption | Pre/post access command execution |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Filesystem | N/A | ✅ | ❌ | ✅ | ✅ |
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

## Backup strategies - features and requirements

### Virtual Machines

### VMware vSphere/ESXi

|  | NBD & HotAdd |
| :--- | :--- |
| Minimum version | 6.x+ |
| Status | In operation \(**preferred**\) |
| Last snapshot kept on hypervisor for inc. backups | yes |
| Hypervisor OS access needed | No |
| Proxy VM needed | no \(optional for HotAdd backup\) |
| Key Caveats | vProtect node must be installed on CentOS/RHEL 7.x to backup VMware virtual machines |

| Feature | NBD & HotAdd |
| :--- | :--- |
| Incremental backup | ✅ |
| File-level restore | ✅ |
| VM disk exclusion | ✅ |
| Snapshot management | ✅ |
| Quiesced snapshot | ❌ \(hypervisor-dependent\) |
| Pre/post snasphot command execution | ✅ |
| Backup disks sharable over iSCSI | ✅ |
| Name-based policy assignment | ✅ |
| Tag-based policy assignment | ✅ |
| Available space for snapshot check | ❌ \(hypervisor-dependent\) |
| Power-on VM after restore | ✅ |

### Microsoft Hyper-V

|  | 2016 & 2019 |
| :--- | :--- |
| Minimum version | Windows Server or Core Hyper-V 2016+ |
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
| Pre/post snasphot command execution | ✅ |
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
| Pre/post snasphot command execution | ✅ |
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
      <td style="text-align:left">4.2+</td>
      <td style="text-align:left">4.2+</td>
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
        <p>addittional snapshot-cloning required</p>
      </td>
      <td style="text-align:left">
        <p>full backup only</p>
        <p>disk attachment process may be slow</p>
      </td>
      <td style="text-align:left">data transfer via Manager</td>
      <td style="text-align:left">access to he hypervisor needed</td>
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
| Pre/post snasphot command execution | ✅ | ✅ | ✅ | ✅ | ✅ |
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
      <td style="text-align:left">4.2+</td>
      <td style="text-align:left">4.2+</td>
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
        <p>addittional snapshot-cloning required</p>
      </td>
      <td style="text-align:left">
        <p>full backup only</p>
        <p>disk attachment process may be slow</p>
      </td>
      <td style="text-align:left">data transfer via Manager</td>
      <td style="text-align:left">access to he hypervisor needed</td>
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
| Pre/post snasphot command execution | ✅ | ✅ | ✅ | ✅ | ✅ |
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
| Pre/post snasphot command execution | ✅ | ✅ | ✅ |
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
| Pre/post snasphot command execution | ✅ | ✅ |
| Backup disks sharable over iSCSI | ❌ | ✅ |
| Name-based policy assignment | ✅ | ✅ |
| Tag-based policy assignment | ❌ | ❌ |
| Available space for snapshot check | ❌ \(hypervisor-dependent\) | ✅ |
| Power-on VM after restore | ✅ | ✅ |

### KVM/Xen

|  | SSH Transfer |
| :--- | :--- |
| Minimum version | QEMU 2.1+ \(qcow2-based VMs need libvirt with blockcommit feature\), libvirt |
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
| Pre/post snasphot command execution | ✅ |
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
| Pre/post snasphot command execution | ✅ | ✅ |
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
| Pre/post snasphot command execution | ❌ |
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
| Pre/post snasphot command execution | ✅ | ✅ |
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
| Pre/post snasphot command execution | ✅ | ✅ |
| Backup disks sharable over iSCSI | ❌ | ✅ |
| Name-based policy assignment | ✅ | ✅ |
| Tag-based policy assignment | ✅ | ✅ |
| Available space for snapshot check | ✅ | ✅ |
| Power-on VM after restore | ✅ | ✅ |

### Containers

### Kubernetes

|  | Name? |
| :--- | :--- |
| Minimum version | 1.10+ |
| Status | In operation \(**preferred**\) |
| Last snapshot kept on hypervisor for inc. backups | Yes |
| Hypervisor OS access needed | no |
| Proxy VM needed | no |

| Feature | Name? |
| :--- | :--- |
| Incremental backup | ✅ \(ceph RBD\) |
| File-level restore | ❌ |
| VM disk exclusion | ✅ |
| Snapshot management | ❌ |
| Quiesced snapshot | ❌ |
| Pre/post snasphot command execution | ❌ |
| Backup disks sharable over iSCSI | ❌ |
| Name-based policy assignment | ✅ |
| Tag-based policy assignment | ✅ |
| Available space for snapshot check | N/A |
| Power-on VM after restore | ✅ |

### Red Hat OpenShift

|  | Name? |
| :--- | :--- |
| Minimum version | 1.10+ |
| Status | In operation \(**preferred**\) |
| Last snapshot kept on hypervisor for inc. backups | Yes |
| Hypervisor OS access needed | no |
| Proxy VM needed | no |

| Feature | Name? |
| :--- | :--- |
| Incremental backup | ✅ \(ceph RBD\) |
| File-level restore | ❌ |
| VM disk exclusion | ✅ |
| Snapshot management | ❌ |
| Quiesced snapshot | ❌ |
| Pre/post snasphot command execution | ❌ |
| Backup disks sharable over iSCSI | ❌ |
| Name-based policy assignment | ✅ |
| Tag-based policy assignment | ✅ |
| Available space for snapshot check | N/A |
| Power-on VM after restore | ✅ |

### Proxmox VE

|  | Name? |
| :--- | :--- |
| Minimum version | 1.10+ |
| Status | In operation \(**preferred**\) |
| Last snapshot kept on hypervisor for inc. backups | Yes |
| Hypervisor OS access needed | no |
| Proxy VM needed | no |

| Feature | Name? |
| :--- | :--- |
| Incremental backup | ✅ \(ceph RBD\) |
| File-level restore | ❌ |
| VM disk exclusion | ✅ |
| Snapshot management | ❌ |
| Quiesced snapshot | ❌ |
| Pre/post snasphot command execution | ❌ |
| Backup disks sharable over iSCSI | ❌ |
| Name-based policy assignment | ✅ |
| Tag-based policy assignment | ✅ |
| Available space for snapshot check | N/A |
| Power-on VM after restore | ✅ |

### Cloud

### AWS EC2

|  | Strategy1 | Strategy2 |
| :--- | :--- | :--- |
| Minimum version | 7.4+ | 7.4+ |
| Status | In operation | In operation \(**preferred**\) |
| Last snapshot kept on hypervisor for inc. backups | yes | yes |
| Hypervisor OS access needed | no | no |
| Proxy VM needed | no | no |
| Key Caveats | file-level restore not supported |  |

| Feature | Strategy1 | Strategy2 |
| :--- | :--- | :--- |
| Incremental backup | ✅ | ✅ |
| File-level restore | ❌ | ✅ |
| VM disk exclusion | ❌ | ✅ |
| Snapshot management | ✅ | ✅ |
| Quiesced snapshot | ✅ | ✅ |
| Pre/post snasphot command execution | ✅ | ✅ |
| Backup disks sharable over iSCSI | ❌ | ✅ |
| Name-based policy assignment | ✅ | ✅ |
| Tag-based policy assignment | ✅ | ✅ |
| Available space for snapshot check | ✅ | ✅ |
| Power-on VM after restore | ✅ | ✅ |

## Conditions / Exclusion

**File-level-restore**:  
_When backing up CentOS 8 with a disk with the GPT partition scheme, you can use file-level restore only on vProtect node, which also runs on CentOS 8 operating system._

**VMware vSphere/ESXi backup:**  
_We can only back up VMware products from a vProtect node that runs on CentOS 7._

