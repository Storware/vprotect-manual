# KVM \(stand-alone\)/Xen \(legacy\) setup

KVM/Xen \(libvirt\) environments require to have correct entry in known\_hosts on the **node**:

* it must be `known_hosts` file that belongs to `vprotect` user
* algorithm must be set to `ssh-rsa`

**Example:**

```text
sudo -u vprotect ssh -o HostKeyAlgorithms=ssh-rsa root@HYPERVISOR
```

* make sure to have in your Node Confiuration `known_hosts` file path set to the location that is accessible for `vprotect` user - default `/opt/vprotect/.ssh/known_hosts`

## KVM - VG scanning

**Used with LVM-based VMs only.** In order to allow VG scanning please make sure to have your LVM packages up to date **on hypervisor**. LVM reporting was one of the features added in RHEL/CentOS 7.3 and is used by vProtect to collect information about VGs. Note that VGs can only be used in restore dialog box when VM was originally deployed with LVM volumes \(not QCOW2\).

## KVM - full libvirt installation

Please make sure to follow steps in this section: [Full versions of libvirt/qemu packages installation](../install/install_libvirt_qemu.md)

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
2. use `ssh-copy-id` to upload your public key \(as `vprotect` user\) to the KVM host:

   ```text
   sudo -u vprotect ssh-copy-id -i /opt/vprotect/.ssh/id_rsa.pub root@HYPERVISOR
   ```

3. Check if you're able to log into the hypervisor using local `vprotect` user without being asked for password:

   ```text
   [root@vProtect3 vprotect]# sudo -u vprotect ssh root@dkvm
   Last failed login: Mon Jan 29 17:53:01 CET 2018 from 10.50.1.107 on ssh:notty
   There was 1 failed login attempt since the last successful login.
   Last login: Mon Jan 29 17:52:39 2018 from 10.50.1.107
   [root@dKVM ~]# logout
   ```

4. Now you should be able to index VMs regardless of the password set for hypervisor \(key should be used instead\).

## 

