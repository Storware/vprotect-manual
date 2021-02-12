# Platform Requirements

## Software Requirements

### Operating Systems

#### RedHat Enterprise Linux / CentOS

Minimum version

* **7.6+**
* **8.1.1911+** _\* vProtect node must be installed on CentOS/RHEL 7.x to backup VMware virtual machines_

Minimal install \(Basic functionality\)

### External packages

#### vProtect server:

MariaDB 10.4

## Hardware Requirements

Minimal requirements:

**CPU:** 2 cores

**RAM:** 4GB

**HDD:** 20GB for OS, and vProtect installation

**HDD:** `X` GB for staging, where **`X`** = `size_of_biggest_VM` \* `number_of_parallel_backups` - optionally can be skipped, when staging space is configured to be directly on external backup destination \(eg. NFS-shared file system\)

## Network Requirements

* Critical for data transfer, understand which paths are used for backups as in many cases you are going to use LAN. Depending on where the node is located you need to verify if data is not going to pass via low-bandwidth links.
* Internet generally not required, but during the installation `yum` needs to fetch packages from the repositories, so you need at least access to your internal repositories.
* Node needs access to the Server \(ports **443** or **8181** depending on the setup\).
* Node needs connectivity with backup providers \(if they are external, such as Power Protect DD\).
* Node needs connectivity with the Hypervisor or Hypervisor Manager.
* If netcat transfer is used for Red Hat Virtualization/oVirt/Oracle Linux VM/Proxmox VE/KVM stand-alone environments - **16000-16999** ports must be reachable from the hypervisors to the node which is responsible for those hypervisors.

### General

| Source | Destination | Ports | Description |
| :--- | :--- | :--- | :--- |
| Node | Server | 443/tcp \(or 8181/tcp\) | Main Node-Server communication over HTTPS \(443, 8181\) |
| Server | Node | 111/tcp, 111/UDP, 2049/tcp, 2049/UDP, ports specified in `/etc/sysconfig/nfs` - variables `MOUNTD_PORT` \(TCP and UDP\), `STATD_PORT` \(TCP and UDP\), `LOCKD_TCPPORT` \(TCP\), `LOCKD_UDPPORT` \(UDP\) | NFS access to browse mountable backups and logs from UI \(using IP that is detected as the source IP - shown in the Node list in UI\) |

### Nutanix AHV

#### Disk attachment

**Connection URL:** `https://PRISM_HOST:9440/api/nutanix/v3` \(Prism Central or Prism Elements\)

**Note:** when connecting via Prism Central, the same credenials will be used to access all Prism Elements

| Source | Destination | Ports | Description |
| :--- | :--- | :--- | :--- |
| Node | Prism Elements \(and optionally Prism Central if used\) | 9440/tcp | API access to the Nutanix manager |

### OpenStack

#### Disk attachment

**Connection URL:** `https://KEYSTONE_HOST:5000/v3`

| Source | Destination | Ports | Description |
| :--- | :--- | :--- | :--- |
| Node | Keystone, Nova, Glance, Cinder | ports that were defined in endpoints for OpenStack services | API access to the OpenStack management services - using endpoint type that has been specified in hypervisor manager details |
| Node | Ceph monitors | 3300/tcp, 6789/tcp | if Ceph RBD is used as the backend storage - used to collect changed-blocks lists from Ceph |

#### SSH transfer

**Connection URL:** `https://KEYSTONE_HOST:5000/v3`

**Note:** you also must provide SSH credentials to all hypervisors that have been detected during inventory sync

| Source | Destination | Ports | Description |
| :--- | :--- | :--- | :--- |
| Node | Hypervisor | 22/tcp | SSH access |
| Hypervisor | Node | netcat port range defined in node configuration - by default 16000-16999/tcp | optional netcat access for data transfer |
| Node | Ceph monitors | 3300/tcp, 6789/tcp, 10809/tcp | if Ceph RBD is used as the backend storage - used for data transfer over NBD |

### oVirt/RHV/OLVM

#### Export storage domain

**Connection URL:** `https://RHV_MGR_HOST/ovirt-engine/api/v3`

| Source | Destination | Ports | Description |
| :--- | :--- | :--- | :--- |
| Node | oVirt/RHV/OLVM manager | 443/tcp | oVirt/RHV/OLVM API access |
| oVirt/RHV/OLVM host selected in export storage domain configuration | Node | If Node is hosting staging space: 111/tcp, 111/UDP, 2049/tcp, 2049/UDP, ports specified in `/etc/sysconfig/nfs` - variables `MOUNTD_PORT` \(TCP and UDP\), `STATD_PORT` \(TCP and UDP\), `LOCKD_TCPPORT` \(TCP\), `LOCKD_UDPPORT` \(UDP\), otherrwise please check documentation of your NFS storage provider | if staging space \(export storage domain\) is hosted on the Node - NFS access |
| Node and oVirt/RHV/OLVM host selected in export storage domain configuration | shared NFS storage | please check documentation of your NFS storage provider | if staging space \(export storage domain\) is hosted on the shared storage - NFS access |

#### Disk attachment

**Connection URL:** `https://MANAGER_HOST/ovirt-engine/api`

| Source | Destination | Ports | Description |
| :--- | :--- | :--- | :--- |
| Node | oVirt/RHV/OLVM manager | 443/tcp | oVirt/RHV/OLVM API access |

#### Disk Image Transfer

**Connection URL:** `https://MANAGER_HOST/ovirt-engine/api`

| Source | Destination | Ports | Description |
| :--- | :--- | :--- | :--- |
| Node | oVirt/RHV/OLVM manager | 443/tcp | oVirt/RHV/OLVM API access |
| Node | oVirt/RHV/OLVM manager | 54322/tcp, 54323/tcp | oVirt/RHV/OLVM ImageIO services - for data transfer |

#### SSH Transfer

**Connection URL:** `https://MANAGER_HOST/ovirt-engine/api`

**Note:** you also must provide SSH credentials to all hypervisors that have been detected during inventory sync

| Source | Destination | Ports | Description |
| :--- | :--- | :--- | :--- |
| Node | oVirt/RHV/OLVM manager | 443/tcp | oVirt/RHV/OLVM API access |
| Node | oVirt/RHV/OLVM hypervisor | 22/tcp | SSH access for data transfer |
| oVirt/RHV/OLVM hypervisor | Node | netcat port range defined in node configuration - by default 16000-16999/tcp | optional netcat access for data transfer |

#### Change-Block Tracking

**Connection URL:** `https://MANAGER_HOST/ovirt-engine/api`

| Source | Destination | Ports | Description |
| :--- | :--- | :--- | :--- |
| Node | oVirt/RHV/OLVM manager | 443/tcp | oVirt/RHV/OLVM API access |
| Node | oVirt/RHV/OLVM hypervisor | 54322/tcp, 54323/tcp | oVirt/RHV/OLVM ImageIO services - for data transfer |
| Node | oVirt/RHV/OLVM manager | 54322/tcp, 54323/tcp | oVirt/RHV/OLVM ImageIO services - for data transfer |

### Oracle VM

#### Export storage domain

**Connection URL:** `https://MANAGER_HOST:7002`

| Source | Destination | Ports | Description |
| :--- | :--- | :--- | :--- |
| Node | OVM manager | 7002/tcp | OVM API access |
| Hypervisor | Node | If Node is hosting staging space: 111/tcp, 111/UDP, 2049/tcp, 2049/UDP, ports specified in `/etc/sysconfig/nfs` - variables `MOUNTD_PORT` \(TCP and UDP\), `STATD_PORT` \(TCP and UDP\), `LOCKD_TCPPORT` \(TCP\), `LOCKD_UDPPORT` \(UDP\), otherrwise please check documentation of your NFS storage provider | if staging space \(export storage repository\) is hosted on the Node - NFS access |
| Node and hypervisor | shared NFS storage | please check documentation of your NFS storage provider | if staging space \(export storage repository\) is hosted on the shared storage - NFS access |

### Citrix XenServer/xcp-ng

**Note:** all hosts in the pool must be defined

#### Single image \(XVA-based\)

| Source | Destination | Ports | Description |
| :--- | :--- | :--- | :--- |
| Node | Hypervisor | 443/tcp | API access \(for data transfer management IP is used, unless `transfer NIC` parameter is configured in hypervisor details\) |

#### Changed-Block Tracking

| Source | Destination | Ports | Description |
| :--- | :--- | :--- | :--- |
| Node | Hypervisor | 443/tcp | API access \(for data transfer management IP is used, unless `transfer NIC` parameter is configured in hypervisor details\) |
| Node | Hypervisor | 10809/tcp | NBD access \(data transfer IP is returned by hypervisor\) |

### KVM/Xen stand-alone

#### SSH transfer

| Source | Destination | Ports | Description |
| :--- | :--- | :--- | :--- |
| Node | Hypervisor | 22/tcp | SSH access |
| Hypervisor | Node | netcat port range defined in node configuration - by default 16000-16999/tcp | optional netcat access for data transfer |
| Node | Ceph monitors | 3300/tcp, 6789/tcp, 10809/tcp | if Ceph RBD is used as the backend storage - used for data transfer over NBD |

### Proxmox VE

#### Export storage repository

| Source | Destination | Ports | Description |
| :--- | :--- | :--- | :--- |
| Node | Hypervisor | 22/tcp | SSH access |
| Hypervisor | Node | If Node is hosting staging space: 111/tcp, 111/UDP, 2049/tcp, 2049/UDP, ports specified in `/etc/sysconfig/nfs` - variables `MOUNTD_PORT` \(TCP and UDP\), `STATD_PORT` \(TCP and UDP\), `LOCKD_TCPPORT` \(TCP\), `LOCKD_UDPPORT` \(UDP\), otherrwise please check documentation of your NFS storage provider | if staging space \(export storage domain\) is hosted on the Node - NFS access |
| Node and hypervisor | shared NFS storage | please check documentation of your NFS storage provider | if staging space \(export storage domain\) is hosted on the shared storage - NFS access |

#### SSH transfer

| Source | Destination | Ports | Description |
| :--- | :--- | :--- | :--- |
| Node | Hypervisor | 22/tcp | SSH access |
| Hypervisor | Node | netcat port range defined in node configuration - by default 16000-16999/tcp | optional netcat access for data transfer |

## Security Requirements

### User Permissions

User `vprotect` must be a member of group "disk".

Sudo privileges are required for the following commands:

**vProtect Node:**

* `/usr/bin/targetcli`
* `/usr/sbin/exportfs`
* `/usr/sbin/kpartx`
* `/usr/sbin/dmsetup`
* `/usr/bin/qemu-nbd`
* `/usr/bin/guestmount`
* `/usr/bin/fusermount`
* `/bin/mount`
* `/bin/umount`
* `/usr/sbin/parted`
* `/usr/sbin/nbd-client`
* `/usr/bin/tee`
* `/opt/vprotect/scripts/vs/privileged.sh`
* `/usr/bin/yum`
* `/usr/sbin/mkfs.xfs`
* `/usr/sbin/fstrim`
* `/usr/sbin/xfs_growfs`
* `/usr/bin/docker`
* `/usr/bin/rbd`
* `/usr/bin/chown`
* `/usr/sbin/nvme`
* `/bin/cp`
* `/sbin/depmod`
* `/usr/sbin/modprobe`
* `/bin/bash`
* `/usr/local/sbin/nbd-client`
* `/bin/make`

**vProtect server:**

* `/opt/vprotect/scripts/application/vp_license.sh`
* `/bin/umount`
* `/bin/mount`

## SELinux

DISABLED - currently it interferes with mountable backups \(file-level restore\) mechanism. Optionally can be changed to PERMISSIVE, or if file-level restore is not required to ENFORCING.

