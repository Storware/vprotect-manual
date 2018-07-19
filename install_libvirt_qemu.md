# Full versions of libvirt/qemu packages installation

Please make sure that your `libvirt` supports `virsh blockcommit` operation. CentOS distribution requires to install full `libvirt` and `qemu-img` from `oVirt` repository. This can be done like this:

1. Install oVirt repo:

   ```
yum install http://resources.ovirt.org/pub/yum-repo/ovirt-release42.rpm -y
   ```

1. Update packages

   ```
yum update -y
   ```
   which should replace `qemu` related packages with full versions from oVirt repo.