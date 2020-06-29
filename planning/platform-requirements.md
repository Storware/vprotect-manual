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

**HDD:** `X` GB for staging, where **`X`** = `size_of_biggest_VM` \* `number_of_parallel_backups`

## Network Requirements

* Critical for data transfer, understand which paths are used for backups as in many cases you are going to use LAN. Depending on where the node is located you need to verify if data is not going to pass via low-bandwidth links.
* Internet generally not required, but during the installation `yum` needs to fetch packages from the repositories, so you need at least access to your internal repositories.
* Node needs access to the Server \(ports **443** and/or **8181** depending on the setup\).
* Node needs connectivity with backup providers \(if they are external, such as Power Protect DD\).
* Node needs connectivity with the Hypervisor or Hypervisor Manager.
* If netcat transfer is used for Red Hat Virtualization/oVirt/Oracle Linux VM environments - **16000-16999** ports must be reachable from the hypervisors to the node which is responsible for those hypervisors.

## Security Requirements

### User Permissions

User `vprotect` must be a member of group "disk".

Sudo privileges are required for a following commands:

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

DISABLED

