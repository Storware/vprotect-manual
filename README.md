# Table of Contents

* [About Storware vProtect](about.md)
* [Overview](overview/)
  * [Architecture](overview/architecture.md)
  * [Licensing](overview/licensing.md)
  * [Main Features](overview/main-features.md)
  * [Typical Scenarios](overview/typical-scenarios.md)
* [Planning](planning/)
  * [vProtect Support Matrix](planning/vprotect-support-matrix.md)
  * [Platform Requirements](planning/platform-requirements.md)
  * [Sizing](planning/sizing/)
    * [Small](planning/sizing/small-environment.md)
    * [Medium](planning/sizing/medium.md)
    * [Large](planning/sizing/large-environment.md)
* [Deployment](deployment/)
  * [Installation Overview](deployment/installation-overview.md)
  * [Quick Install \(All-In-One\)](deployment/quick-install-all-in-one.md)
  * [Installation using Ansible playbook](deployment/installation-using-ansible-playbook.md)
  * [Installation with RPMs](deployment/installation-with-rpms.md)
  * [Virtual Appliance](deployment/virtual-appliance/)
    * [RHV/oVirt/OLVM Virtual Appliance](deployment/virtual-appliance/rhv-ovirt-olvm-virtual-appliance.md)
    * [Citrix Hypervisor \| XCP-ng Virtual Appliance](deployment/virtual-appliance/citrix-hypervisor-or-xcp-ng-virtual-appliance.md)
    * [VMware Virtual Appliance](deployment/virtual-appliance/vmware-virtual-appliance.md)
  * [Initial Configuration](deployment/initial-configuration.md)
  * [Protected Platforms](deployment/protected-platforms/)
    * [Virtual Machines](deployment/protected-platforms/virtual-machines/)
      * [VMware vSphere](deployment/protected-platforms/virtual-machines/vmware-vsphere.md)
      * [Microsoft Hyper-V](deployment/protected-platforms/virtual-machines/microsoft-hyper-v.md)
      * [Nutanix Acropolis Hypervisor \(AHV\)](deployment/protected-platforms/virtual-machines/nutanix-acropolis-ahv.md)
      * [Red Hat Virtualization](deployment/protected-platforms/virtual-machines/red-hat-virtualization.md)
      * [oVirt](deployment/protected-platforms/virtual-machines/ovirt.md)
      * [Oracle Linux Virtualization Manager](deployment/protected-platforms/virtual-machines/oracle-linux-virtualization-manager.md)
      * [Proxmox VE](deployment/protected-platforms/virtual-machines/proxmox-ve.md)
      * [KVM/Xen](deployment/protected-platforms/virtual-machines/kvm-xen.md)
      * [OpenStack](deployment/protected-platforms/virtual-machines/openstack.md)
      * [Oracle VM](deployment/protected-platforms/virtual-machines/oracle-vm.md)
      * [Citrix Hypervisor \(XenServer\)](deployment/protected-platforms/virtual-machines/citrix-hypervisor-xenserver.md)
      * [XCP-ng](deployment/protected-platforms/virtual-machines/xcp-ng.md)
    * [Containers](deployment/protected-platforms/containers/)
      * [Kubernetes](deployment/protected-platforms/containers/kubernetes.md)
      * [Red Hat OpenShift](deployment/protected-platforms/containers/red-hat-openshift.md)
      * [Proxmox VE](deployment/protected-platforms/containers/proxmox-ve.md)
    * [Cloud](deployment/protected-platforms/cloud/)
      * [AWS EC2](deployment/protected-platforms/cloud/aws-ec2.md)
    * [Applications](deployment/protected-platforms/applications.md)
  * [Backup Destinations](deployment/backup-destinations/)
    * [Filesystem](deployment/backup-destinations/filesystem/)
      * [Regular filesystem](deployment/backup-destinations/filesystem/regular-filesystem.md)
      * [Virtual Data Optimizer \(VDO\)](deployment/backup-destinations/filesystem/virtual-data-optimizer-vdo.md)
    * [Deduplication Appliances](deployment/backup-destinations/deduplication-appliances/)
      * [Dell EMC Data Domain](deployment/backup-destinations/deduplication-appliances/dell-emc-data-domain.md)
      * [HPE StoreOnce](deployment/backup-destinations/deduplication-appliances/hpe-storeonce.md)
      * [Exagrid](deployment/backup-destinations/deduplication-appliances/exagrid.md)
      * [Neverfail HybriStor](deployment/backup-destinations/deduplication-appliances/neverfail-hybristor.md)
      * [Catalogic Software vStor](deployment/backup-destinations/deduplication-appliances/catalogic-software-vstor.md)
    * [Object Storage](deployment/backup-destinations/object-storage/)
      * [AWS S3](deployment/backup-destinations/object-storage/aws-s3.md)
      * [Google Cloud Storage](deployment/backup-destinations/object-storage/google-cloud-storage.md)
      * [Microsoft Azure Blob Storage](deployment/backup-destinations/object-storage/microsoft-azure-blob-storage.md)
      * [OpenStack SWIFT](deployment/backup-destinations/object-storage/openstack-swift.md)
      * [IBM Cloud Object Storage](deployment/backup-destinations/object-storage/ibm-cloud-object-storage.md)
      * [Oracle Cloud Infrastructure Object Storage](deployment/backup-destinations/object-storage/oracle-cloud-infrastructure-object-storage.md)
      * [Scality RING](deployment/backup-destinations/object-storage/scality-ring.md)
      * [Ceph Rados Gateway](deployment/backup-destinations/object-storage/ceph-rados-gateway.md)
    * [Enterprise Backup Providers](deployment/backup-destinations/enterprise-backup-providers/)
      * [Dell EMC Avamar](deployment/backup-destinations/enterprise-backup-providers/dell-emc-avamar.md)
      * [Dell EMC Networker](deployment/backup-destinations/enterprise-backup-providers/dell-emc-networker.md)
      * [IBM Spectrum Protect](deployment/backup-destinations/enterprise-backup-providers/ibm-spectrum-protect.md)
      * [Veritas Netbackup](deployment/backup-destinations/enterprise-backup-providers/veritas-netbackup.md)
  * [Integrations Plugins](deployment/integrations-plugins/)
    * [Red Hat Virtualization UI Plugin](deployment/integrations-plugins/redhat-virtualization-plugin.md)
    * [oVirt UI Plugin](deployment/integrations-plugins/ovirt-plugin.md)
    * [Oracle Linux Virtualization Manager UI Plugin](deployment/integrations-plugins/oracle-linux-virtualization-manager-plugin.md)
    * [OpenStack UI Plugin](deployment/integrations-plugins/openstack-plugin.md)
  * [Common Tasks](deployment/common-tasks/)
    * [Staging space configuration](deployment/common-tasks/staging-space-configuration.md)
    * [Enabling HTTPS connectivity for remote nodes](deployment/common-tasks/enabling-https-connectivity-for-remote-nodes.md)
    * [LVM setup on vProtect Node for disk attachment backup mode](deployment/common-tasks/lvm-setup-on-vprotect-node-for-disk-attachment-backup-mode.md)
    * [Full versions of libvirt/qemu packages installation](deployment/common-tasks/full-versions-of-libvirt-qemu-packages-installation.md)
    * [SSH public key authentication](deployment/common-tasks/ssh-public-key-authentication.md)
* [Administration](administration/)
  * [Dashboard](administration/dashboard.md)
  * [Backup](administration/backup/)
    * [On-Demand Backup](administration/backup/on-demand-backup.md)
    * [Scheduled Backup](administration/backup/scheduled-backup.md)
  * [Restore](administration/restore/)
    * [On-Demand Restore](administration/restore/on-demand-restore.md)
    * [Scheduled \(Recovery Plans\)](administration/restore/scheduled-recovery-plans.md)
  * [Snapshot Management](administration/snapshot/)
    * [On-Demand](administration/snapshot/on-demand-snapshot.md)
    * [Scheduled](administration/snapshot/scheduled-snapshot.md)
  * [File-level Restore \(Mounted Backup\)](administration/file-level-restore-mounted-backup.md)
  * [Policies](administration/policies/)
    * [Virtual Environment Backup](administration/policies/virtual-environments-backup.md)
    * [VM Snapshot Management](administration/policies/vm-snapshot-management.md)
    * [Application Backup](administration/policies/application-backup.md)
    * [VM Recovery Plans](administration/policies/vm-recovery-plans.md)
  * [Schedules](administration/schedules/)
    * [Virtual Environments Schedules](administration/schedules/virtual-environments-schedules.md)
    * [Snapshots Schedules](administration/schedules/snapshots-schedules.md)
    * [Application Backup](administration/schedules/application-backup.md)
    * [Recovery Plans Schedules](administration/schedules/recovery-plans-schedules.md)
  * [Nodes](administration/nodes.md)
  * [Disaster Recovery](administration/disaster-recovery.md)
  * [Upgrade](administration/upgrade.md)
  * [User Management](administration/users.md)
  * [Audit Log](administration/audit-log.md)
  * [Reporting](administration/reporting.md)
  * [Settings](administration/settings.md)
  * [CLI Reference](administration/cli-reference.md)
* [Troubleshooting](troubleshooting/)
  * [How to enable vProtect DEBUG mode](troubleshooting/how-switch-vprotect-to-debug-mode.md)
  * [Collecting logs](troubleshooting/collecting-logs.md)
* [Integration](api-integration-guide.md)
* [Known software issues and limitations](known-software-issues-and-limitations.md)

