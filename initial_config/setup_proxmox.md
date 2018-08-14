# Proxmox setup

Proxmomx environments require backup storage to be defined on each server. This storage must be location accessible from vProtect Node \(the simplest setup, when you use only 1 node, is to create NFS share for staging path on vProtect Node\)

1. Create storage from NFS share \(Content type: VZDump\)

![](../.gitbook/assets/setup_proxmox-editstorage%20%281%29.png)

   * Note that export must be set to use UID and GID of `vprotect` user
   * Example export configuration in `/etc/exports` to the selected hypervisor in RHV cluster:

     ```text
     /vprotect_data    PROXMOX_HOSTS(fsid=6,rw,sync,insecure,all_squash,anonuid=993,anongid=990)
     ```

     where `anonuid=993` and `anongid=990` should have correct UID and GID returned by command:

     ```text
     [root@vProtect3 ~]# id vprotect
     uid=993(vprotect) gid=990(vprotect) groups=990(vprotect)
     ```

2. Both import and export operations will be done using this NFS shares – restore will be done directly to this storage domain, so you can easily import backup into Proxmox environment

   * backups must be restored to the export path \(node automatically changes names to the original paths that are recognized by Proxmox.

![](../.gitbook/assets/setup_proxmox-storagelist.png)

3. Name for storage must be later provided in node configuration \(`Hypervisor -> Proxmox` section\)
4. Proxmomx \(like KVM/Xen\) environments require to have correct entry in known\_hosts on the node:

   * it must be known\_hosts file that belongs to `vprotect` user
   * algorithm must be set to `ssh-rsa`

   **Example:**

   ```text
   sudo -u vprotect ssh -o HostKeyAlgorithms=ssh-rsa root@HYPERVISOR
   ```

   * make sure to have in your Node Confiuration `known_hosts` file path \(in `KVM/Xen` section\) set to the location that is accessible for `vprotect` user - default `/opt/vprotect/.ssh/known_hosts`

## Public key authentication

Instead of using password authentication you also have public key alternative.

**Example:**

1. Generate key or use yours and store it as `/opt/vprotect/.ssh/id_rsa` \(make sure that `vprotect` user and group own the file\)
   * example key generation:

     ```text
     [root@vProtect3 vprotect]# sudo -u vprotect ssh-keygen
     Generating public/private rsa key pair.
     Enter file in which to save the key (/opt/vprotect/.ssh/id_rsa): 
     Enter passphrase (empty for no passphrase): 
     Enter same passphrase again: 
     Your identification has been saved in /opt/vprotect/.ssh/id_rsa.
     Your public key has been saved in /opt/vprotect/.ssh/id_rsa.pub.
     The key fingerprint is:
     SHA256:86HSLKYwl7maDR7U1oIH1Y6VDtRFNJgHgfdjikg3VnQ vprotect@vProtect3
     The key's randomart image is:
     +---[RSA 2048]----+
     |   .o=+XE        |
     |   .o X...       |
     |  .  O o         |
     |  .+=.o +        |
     | .o+=o.oS..      |
     | ..o.+.o + .     |
     |  = + + + .      |
     | . O + o         |
     |  +.+            |
     +----[SHA256]-----+
     ```
2. use `ssh-copy-id` to upload your public key \(as `vprotect` user\) to the Proxmox host:

   ```text
   sudo -u vprotect ssh-copy-id -i /opt/vprotect/.ssh/id_rsa.pub root@HYPERVISOR
   ```

3. Check if you're able to log into the hypervisor using local `vprotect` user without being asked for password:

   ```text
   [root@vProtect3 vprotect]# sudo -u vprotect ssh root@proxmox
   Last failed login: Mon Jan 29 17:54:01 CET 2018 from 10.50.1.107 on ssh:notty
   There was 1 failed login attempt since the last successful login.
   Last login: Mon Jan 29 17:54:39 2018 from 10.50.1.107
   [root@proxmox ~]# logout
   ```

4. Now you should be able to index VMs regardless of the password set for hypervisor \(key should be used instead\).

