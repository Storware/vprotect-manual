# Protected Platforms

vProtect supports multiple virtualization platforms. In this section you will find out what specific steps are needed for each of them to be integrated with vProtect.

* [Virtual Machines](virtual-machines/)
  * [VMware vSphere](virtual-machines/vmware-vsphere.md)
  * [Microsoft Hyper-V](virtual-machines/microsoft-hyper-v.md)
  * [Nutanix Acropolis Hypervisor \(AHV\)](virtual-machines/nutanix-acropolis-ahv.md)
  * [Red Hat Virtualization](virtual-machines/red-hat-virtualization.md)
  * [oVirt](virtual-machines/ovirt.md)
  * [Oracle Linux Virtualization Manager](virtual-machines/oracle-linux-virtualization-manager.md)
  * [Proxmox VE](virtual-machines/proxmox-ve.md)
  * [KVM/Xen](virtual-machines/kvm-xen.md)
  * [OpenStack](virtual-machines/openstack.md)
  * [Oracle VM](virtual-machines/oracle-vm.md)
  * [Citrix Hypervisor \(XenServer\)](virtual-machines/citrix-hypervisor-xenserver.md)
  * [XCP-ng](virtual-machines/xcp-ng.md)
  * [Huawei FusionCompute](virtual-machines/huawei-fusion-compute.md)
  * [HPE SimpliVity](virtual-machines/hpe-simplivity.md)
* [Containers](containers/)
  * [Kubernetes](containers/kubernetes.md)
  * [Red Hat OpenShift](containers/red-hat-openshift.md)
  * [Proxmox VE](containers/proxmox-ve.md)
* [Cloud](cloud/)
  * [AWS EC2](cloud/aws-ec2.md)
* [Applications](applications.md)

In all the methods described in this section, it is always assumed that the vProtect Node only communicates with the protected platform and server reponsible for configuration and metadata management. This means that:

* it is always the Node that requires access to the infrastructure \(protected platforms APIs, storage etc., guest VMs for iSCSI shares, etc.\)
* the server only needs to be accessible to the Nodes and admin browser over HTTP/HTTPS \(which means that it can usually reside anywhere in the infrastructure\)

