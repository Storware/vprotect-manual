# Full versions of libvirt/qemu packages installation

Please make sure that your `libvirt` supports the `virsh blockcommit` operation. CentOS distribution requires you to install the full `libvirt` and `qemu-img` from the `oVirt` repository. This can be done like this:

1. Install oVirt repo:

   ```text
   yum install http://resources.ovirt.org/pub/yum-repo/ovirt-release42.rpm -y
   ```

2. Update the packages

   ```text
   yum update -y
   ```

   which should replace `qemu` related packages with full versions from the oVirt repo.

