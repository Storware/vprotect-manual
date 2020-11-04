# Protected Platforms

vProtect supports multiple virtualization, container and cloud platforms. In this section you will find what specific steps are needed for each one of them to be integrated with vProtect.

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
* [Containers](containers/)
  * [Kubernetes](containers/kubernetes.md)
  * [Red Hat OpenShift](containers/red-hat-openshift.md)
  * [Proxmox VE](containers/proxmox-ve.md)
* [Cloud](cloud/)
  * [AWS EC2](cloud/aws-ec2.md)
* [Applications](applications.md)

In all methods described in this section it is always assumed that vProtect Node communicates with the protected platform and server only is responsible for configuration and metadata management. This means that:

* it is always Node that requires access to the infrastructure \(protected platforms APIs, storage etc., guest VMs for iSCSI shares, etc.\)
* server only needs to be accessible by the Nodes and admin browser over HTTP/HTTPS \(which means that it can reside usually anywhere in the infrastructure\)

