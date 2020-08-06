# vProtect Support Matrix

## Virtualization Platform Support

| Virtualization Platform | Supported Version |
| :--- | :--- |
| VMware vSphere/ESXi | 5.5+, 6.x+, 7.0 |
| Microsoft Hyper-V | 2016, 2019 |
| Nutanix Acropolis \(AHV\) | 5.5+ \(Intel-only\) |
| Red Hat Virtualization | 3.5.1+ \(RHEL 7.1+\) |
| oVirt | 3.5.1+ \(RHEL/CentOS 7.1+\) |
| Oracle Linux Virtualization Manager | 3.5.1+ \(Oracle Linux 7.1+\) |
| Proxmox VE | 5+ |
| KVM/Xen | QEMU 2.1+ \(qcow2-based VMs need libvirt with block commit feature\) |
| OpenStack | QEMU 2.1+ \(qcow2-based VMs need libvirt with block commit feature\) |
| Oracle VM | 3.4+ |
| Citrix Hypervisor \(XenServer\) | 6.5+ |
| XCP-ng | 6.5+ |
| HPE SimpliVity | Gen9+ |

## Container Platform Support

| Conta Platform | Supported Version |
| :--- | :--- |
| Kubernetes \(K8s\) | 1.10+ |
| Red Hat OpenShift | 3.11+ |
| Proxmox VE | 5+ |

## Cloud Platform Support

| Cloud Platform | Supported Version |
| :--- | :--- |
| Amazon EC2 instances | Current \(AWS 2020\) |

## Backup Destination \(FileSystem\) Support

| Backup  Provider | Supported version | Random Access | Deduplication | Encryption | pre/post   access command execution |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Filesystem | N/A | Yes | No | Yes | Yes |
| Dell EMC Data Domain | DD OS 7.1.0 | Yes | Yes | Yes | Yes |

## Backup Destination \(Object Storage\) Support

| Backup  Provider | Supported version | Random Access | Deduplication | Encryption | pre/post   access command execution |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Amazon S3 | Current | No | N/A | Yes | Yes |
| S3 compatible | Current | No | N/A | Yes | Yes |
| MS Azure Blob Storage | Current | No | N/A | Yes | Yes |
| OpenStack SWIFT | API v2 | No | N/A | No | Yes |
| Scality RING | 6.4+ | No | N/A | Provider-dependent | Yes |

## Backup Destination \(Enterprise backup providers\) Support

| Backup  Provider | Supported version | Random Access | Deduplication | Encryption | pre/post   access command execution |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Dell EMC Avamar | 7.5+ | No | Yes | Provider-dependent | Yes |
| Dell EMC Networker | 9+ | No | Yes | Provider-dependent | Yes |
| IBM Spectrum Protect | 7.1.5+ | No | Yes | Provider-dependent | Yes |
| Veritas Netbackup | 7.6+ | No | Yes | Provider-dependent | Yes |

## Feature support table

| Virtualization Platform | Incremental backup | File-Level-Restore\* | VM disk exclusion | Snapshot Management | Pre/Post Snapshot command execution | VM Tags |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| VMware vSphere/ESXi | 5.5+ | yes | yes | yes | yes | yes |
| Microsoft Hyper-V | 2016+ | yes | yes | yes | yes | yes |
| Red Hat Virtualization | 4.2+ | yes | yes \(APIv4\) | yes | yes | yes |
| oVirt | 4.2+ | yes | yes \(APIv4\) | yes | yes | yes |
| Oracle Linux Virtualization Manager | 4.3+ | yes | yes \(APIv4\) | yes | yes | yes |
| Proxmox VE | no | yes | yes | yes | yes | no |
| KVM/Xen | yes | yes | yes | no | yes | no |
| OpenStack | yes | yes | yes | no | yes | yes |
| Oracle VM | no | yes | yes | no | no | no |
| Citrix Hypervisor \(XenServer\) | yes | yes \(CBT mode\) | yes \(CBT mode\) | yes | yes | yes |
| XCP-ng | yes | yes \(CBT mode\) | yes \(CBT mode\) | yes | yes | yes |
| AWS EC2 | no | yes | yes | yes | no | yes |
| Kubernetes | Yes \(for Ceph RBD based storage\) | no | yes | no | Yes \(post-export cmd. exec.\) | planned |
| Red Hat OpenShift | Yes \(for Ceph RBD based storage\) | no | yes | no | Yes \(post-export cmd. exec.\) | planned |

### Conditions / Exclusion

**File-level-restore**:  
_When backing up CentOS 8 with a disk with a GPT partition scheme, you can use file-level restore only on vProtect node, which also runs on CentOS 8 operating system._

