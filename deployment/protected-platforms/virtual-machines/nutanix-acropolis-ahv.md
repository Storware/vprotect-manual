# Nutanix Acropolis Hypervisor \(AHV\)



vProtect supports Nutanix AHV platform by using a VM called “Proxy VM”. The node invokes commands on your hypervisor manager to snapshot and attach drives of a specific VM to itself \(Proxy VM\). The proxy VM is able to read the data from the attached disk snapshots and forward them to the backup provider.

This strategy allows you to exclude drives from backup that you do not need. Remember that, you need to install 1 Proxy VM per cluster, so that the drives that the node tries to attach are reachable.

**Note**: staging space must be on a volume coming from container storage. Otherwise vProtect may select the wrong device during backup.

![](https://github.com/backupmonster/storware-vprotect-manual/tree/31778b5e60e67956cc3fb965d118537bb2d2be7e/.gitbook/assets/dell-emc-vprotect_nutanix-disk-attachment.png)

**Note**: Nutanix environments require the vProtect node to be installed in one of the VMs residing on the Nutanix cluster. vProtect should automatically detected the VM with vProtect during the index operation.

vProtect requires that there be a user with "cluster admin" privileges on Prism, to process the backup/restore job.

Follow these steps: [LVM setup on vProtect Node for disk attachment backup mode](../../common-tasks/lvm-setup-on-vprotect-node-for-disk-attachment-backup-mode.md)

When adding Nutanix hypervisor managers \(Prism Elements\) use a URL similar to the following:

```text
https://PRISM_HOST:9440/api/nutanix/v3
```

Then index your HV manager and verify if hypervisors and VMs are detected.

**Note**: volume groups attached to the VMs are **not** affected by snapshot, hence neither backup nor snapshot revert on such volumes is going to include them.

**Note**: staging space must be on a volume coming from container storage. Otherwise vProtect may select the wrong device during backup.

