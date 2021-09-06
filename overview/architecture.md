# Architecture

## High-level Architecture

Use vProtect to back up data from your virtualization platforms. You can back up data to and recover data from local filesystem or an NFS/CIFS share, object storage \(cloud providers\) or enterprise backup providers like Dell EMC, IBM, Veritas.

![](../.gitbook/assets/vprotect_architecture_4_1_january_2021.png)

## Detailed Architecture

![](../.gitbook/assets/vprotect_4_1_detailed_architecture.png)

vProtect consists of 2 main components:

* **vProtect Server** - central point of vProtect management, provides administrative Web UI, APIs and is a central repository of metadata.
* **vProtect Node** - data mover that performs backups, restores and mounts:
  * multiple nodes can be deployed for scalability or other reasons,
  * all nodes are managed by the server and need to be registered to the server.

## Component placement

* **vProtect Server and Node can be installed on the same host** 
* The Server can be installed on a physical machine or VM - externally deployed nodes require network connectivity to the Server and PowerProtect DD target\(s\).
* Nodes may be deployed as physical or virtual systems, unless selected backup strategy requires the Node to be installed as a VM on a Hypervisor Cluster \(especially when the "disk attachment" export mode is mentioned\).
* Both components are installed on a CentOS/RHEL 8 minimal.

For detailed deployment scenarios refer to the following sections:

### Virtual Machines

* [Deployment on VMware vSphere/ESXi](../deployment/protected-platforms/virtual-machines/vmware-vsphere.md)
* [Deployment on Hyper-V](../deployment/protected-platforms/virtual-machines/microsoft-hyper-v.md)
* [Deployment on Nutanix Acropolis \(AHV\)](../deployment/protected-platforms/virtual-machines/nutanix-acropolis-ahv.md)
* [Deployment on Red Hat Virtualization](../deployment/protected-platforms/virtual-machines/red-hat-virtualization.md)
* [Deployment on oVirt](../deployment/protected-platforms/virtual-machines/ovirt.md)
* [Deployment on Oracle Linux Virtualization Manager](../deployment/protected-platforms/virtual-machines/oracle-linux-virtualization-manager.md)
* [Deployment on Proxmox VE ](../deployment/protected-platforms/virtual-machines/proxmox-ve.md)
* [Deployment on KVM/Xen](../deployment/protected-platforms/virtual-machines/kvm-xen.md)
* [Deployment on OpenStack](../deployment/protected-platforms/virtual-machines/openstack.md)
* [Deployment on Oracle VM ](../deployment/protected-platforms/virtual-machines/oracle-vm.md)
* [Deployment on Citrix Hypervisor \(XenServer\)](../deployment/protected-platforms/virtual-machines/citrix-hypervisor-xenserver.md)
* [Deployment on XCP-ng](../deployment/protected-platforms/virtual-machines/xcp-ng.md)
* [Deployment on HPE SimpliVity](../deployment/protected-platforms/virtual-machines/hpe-simplivity.md)

### Containers

* [Deployment on Kubernetes](../deployment/protected-platforms/containers/kubernetes.md)
* [Deployment on Red Hat OpenShift ](../deployment/protected-platforms/containers/red-hat-openshift.md)
* [Deployment on Proxmox VE](../deployment/protected-platforms/containers/proxmox-ve.md)

### Cloud

* [Deployment on AWS EC2](../deployment/protected-platforms/cloud/aws-ec2.md)

### Storage Providers

* [File System](../deployment/protected-platforms/storage-providers/file-system.md)
* [Ceph RBD](../deployment/protected-platforms/storage-providers/ceph-rbd.md)
* [Nutanix Files](../deployment/protected-platforms/storage-providers/nutanix-files.md)

## Typical workflows

There are several standard workflows in vProtect and they result in a set of tasks:

* **Backup**
  * **Export** - task that creates snapshot and exports data to the staging space
  * **Store** - task that moves data to the backup destination
* **Restore to filesystem**
  * **Restore** - a task that gets data from a backup provider and puts data in the staging space
* **Restore to virtualization platform**
  * **Restore** - a task that gets data from a backup provider and puts data in the staging space \(if it is a full backup that is being restored residing on the file system backup provider - this task just informs where files are waiting for import task\)
  * **Import** - task that imports data to the virtualization platform and recreates VM
* **Restore for mount \(file-level restore\)**
  * **Restore** - a task that gets data from a backup provider and puts data in the staging space \(if it is a full backup that is being restored residing on the file system backup provider - this task just informs where files are waiting for mount task\)
  * **Mount** - mounts backup on the vProtect Node and either allows user to browse files or exposes backup over iSCSI, so that remote iSCSI initiator can access it\)
* **Snapshot**
  * **Snapshot** - a task that creates a local persisted snapshot of a VM in the hypervisor environment according to a policy that was assigned to the VM - snapshots that are no longer needed \(according to the policy\) will be removed

![](../.gitbook/assets/workflows.png)

