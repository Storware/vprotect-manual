# Huawei FusionCompute

* Supported version: 8.x . 
* The node can be installed outside of the environment, but not behind a NAT.
* Incremental backup uses the CBT functionality of VRM. This means that currently, incremental backup can only be performed on a vms with installed VMTools \(You can find instructions on how to install VMTools on VRM's Help page\).  

![](../../../.gitbook/assets/vProtect_FusionCompute-CBT.png)

## Creating a FusionCompute HypervisorManager

* Fill in the url field:
  * https
  * address of VRM server, hostname or IP, with port \(default is 8443\)
* Fill in the admin username and password for VRM.

![](../../../.gitbook/assets/protected-platforms-fusioncompute.png)

## Backup and Restore

* Backup and restore use LANSSL transfer mode for communication with CNAs. 
* Requires a connection to each Hypervisor \(data transfer during the backup/restore process will be performed directly between the node and the CNA/Hypervisor on which the vm is running\).

### Backup process

* A snapshot of the VM is performed. \(type 'normal' when the vm is without VMTools or type 'CBTbackup' if the vm has VMTools installed\).
* The metadata of vm is backed up in json format.
* Each disk is then backed up sequentially, using LANSSL transfer mode. During this process, the node communicates with the specifc CNA directly. 
* If incremental backup is performed: additionally the CBT map for each disk is backed up.
* The last snasphot is kept on the hvm for incremental backup.

### Restore process

* The node sends a request to the VRM to create a new VM based on the metadata stored during the backup process. The VRM automatically creates the required disks.
* For each disk, the node sends data using LANSSL transfer mode directly to the CNA, restoring the backed up content. 

### Restore settings

\*Restore to hypervisor manager

* Select the storage to which disks will be restored
* Cluster and host selection:
  * If the host is selected then the vm will be restored to that host and bound to it.
  * If the host is set to None, then a cluster must be selected and the vm will be restored to that cluster
* You can specify the name for the restored vm.

![](../../../.gitbook/assets/protected-platforms-fusioncompute-restore.png)
