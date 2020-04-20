# KVM \(stand-alone\)/Xen \(legacy\) setup

KVM/Xen \(libvirt\) environments require to have correct entry in known\_hosts on the **node**:

* it must be `known_hosts` file that belongs to `vprotect` user
* algorithm must be set to `ssh-rsa`

**Example for older versions \(pre 3.8\):**

```text
sudo -u vprotect ssh -o HostKeyAlgorithms=ssh-rsa root@HYPERVISOR
```

* make sure to have in your Node Confiuration `known_hosts` file path set to the location that is accessible for `vprotect` user - default `/opt/vprotect/.ssh/known_hosts`
* if your user/group used on KVM host is other than `qemu:qemu` then please provide them in hypervisor details form when you add/update hypervisor

## KVM - VG scanning

**Used with LVM-based VMs only.** In order to allow VG scanning please make sure to have your LVM packages up to date **on hypervisor**. LVM reporting was one of the features added in RHEL/CentOS 7.3 and is used by vProtect to collect information about VGs.

## KVM - full libvirt installation

Please make sure to follow steps in this section: [Full versions of libvirt/qemu packages installation](../../install/install_libvirt_qemu.md)

## Public key authentication

Details described in [SSH public key authentication](../../install/ssh-public-key-authentication.md) section.

