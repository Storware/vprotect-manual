# Proxmox VE

Proxmox environments require backup storage to be defined on each server. This storage must be location accessible from vProtect Node \(the simplest setup, when you use only 1 node, is to create NFS share for staging path on vProtect Node\)

1. Create storage from NFS share \(Content-type: VZDump\)

![](../../../.gitbook/assets/containers-proxmox-ve-nfs-share.jpg)

* Export share must be set to use UID and GID of `vprotect` user
* Example export configuration in `/etc/exports` to the selected hypervisor in RHV cluster:

  ```text
  /vprotect_data    PROXMOX_HOSTS(fsid=6,rw,sync,insecure,all_squash,anonuid=993,anongid=990)
  ```

  where `anonuid=993` and `anongid=990` should have correct UID and GID returned by command:

  ```text
  [root@vProtect3 ~]# id vprotect
  uid=993(vprotect) gid=990(vprotect) groups=990(vprotect)
  ```

* Both import and export operations will be done using these NFS shares – restore will be done directly to this storage domain, so you can easily import backup into the Proxmox environment
  * backups must be restored to the export path \(node automatically changes names to the original paths that are recognized by Proxmox.
* Name for storage must be later provided in node configuration \(`Hypervisor -> Proxmox` section\)

![](../../../.gitbook/assets/containers-proxmox-ve-storage%20%283%29%20%282%29%20%281%29%20%281%29.jpg)

## File-level restore support for VMA images

1. Prepare VMA extractor on vProtect Node - you have 2 options:
   * build VMA extractor like this \(requires Internet on the **node**\):

     ```text
     cd /opt/vprotect/scripts/vma
     ./setup_vma.sh
     ```
2. * download VMA extractor from Storware FTP \(in Add-ons directory\) and install it

     ```text
     cd /opt/vprotect/scripts/vma
     ./setup_vma.sh PATH_TO_VMA_ARCHIVE
     ```

## Public key authentication

Details described in [SSH public key authentication](../../common-tasks/ssh-public-key-authentication.md) section.

