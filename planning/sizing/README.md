# Sizing Guide

There are two types of people: those who are doing backups, and those who will be doing them. But just **doing** backups is not everything. To avoid unnecessary surprises the best strategy is to **plan** your backup environment/procedure **before implementing** it. In this chapter, we have collected generic hints and guides which you might find useful while thinking about your **vProtect** implementation.

1. Collect information about the `TotalSizeOfData` to be protected in your environment
   * this is the size of your VMs/Storage that will be transferred within the backup window
     * for general sizing, assume all backups to be full
   * if your staging space is separate from the backup destination, please also check what are the biggest VMs/Storage that may end up in your staging area 
2. 2. Assume `BackupWindow` length - backups are usually executed overnight, so 10h-12h is common practice
3. Run a **test transfer** on a test file to estimate maximum achievable bandwidth per thread \(`SingleThreadTransfer`\) from the hypervisor \(or manager\) to the node
   * we recommend 10 simultaneous transfers with the result divided by 10 threads to see if other limitations of the environment do not impact the total transfer rate \(one such common limitation is disk read performance on the virtualization platform\)
   * all the methods usually use snapshots to do backup - please check if snapshot removal in your environment does not take a significant amount of time, as it is a highly resource-intensive operation that impacts overall backup time - especially when running multiple export jobs in parallel
4. Estimate **the** **number of the nodes**
   * required bandwidth per node:

      `RequiredBandwidth = TotalSizeOfData / BackupWindow`

   * the total number of export tasks \(note that other aspects such as snapshot handling, file system scanning during export, and infrastructure bottlenecks when using multiple threads will usually impact the overall speed\):

     `TotalNumberOfExportTasks = RequiredBandwidth / (70% * SingleThreadTransferSpeed)`

   * the number of nodes:

     `NumberOfTheNodes = TotalNumberOfExportTasks / 10` **\(10 is the recommended maximum number of export tasks per Node\)**

   * note that the `TotalSizeOfData` does not mean that it is only a full backup, as you can mix full and incremental backups
   * granularity is a single hypervisor or storage provider, so at the maximum you cannot have more nodes than hypervisor storage providers in your environment
   * if you have multiple clusters and you want to use the disk-attachment method, this automatically implies a minimum of 1 node per cluster
5. Estimate the total **store rate** in the backup destination
   * if multiple nodes are required, add up the total amount of data from all nodes
   * if your backup destination is accessible over LAN
     * do a test transfer from the node to the backup destination to verify if the performance on the backup destination is able to receive such a load
6. `NumberOfExportTasksPerNode`
   * we recommend using the same node configuration for multiple nodes, so the same limit value will be applied to all nodes sharing the configuration
   * this implies that we recommend assuming this value as follows \(rounded down\):
     `NumberOfExportTasksPerNode = TotalNumberOfExportTasks / NumberOfTheNodes`
7. `NumberOfStoreTasksPerNode` usually depends on destination backup performance
   * we recommend a value equal to the `NumberOfExportTasksPerNode` or higher
   * reduce this value only if your backup provider starts to have significant I/O latency eventually leading to a slower write rate than with the lower number of threads - this will typically result in higher staging space occupation as backups will be kept for a longer period of time in the temporary space
8. **Node resource requirements:**
   * **CPU**: Assume 0.5 CPU per task, minimum 2 cores - rounded up to get the full core count supported by the hypervisor or physical server \(i.e. it may be required to round up 2.5 cores to 4 vCPUs if the hypervisor on which the node is deployed doesn't allow to 3 vCPUs to be assigned\)
     * if SSH transfer \(without netcat\) or client-side deduplication is used \(i.e. VDO\) assume 1 CPU per task
   * **Memory**: 256 MB RAM per task, with a minimum of 2GB
+   * **Staging space:** if not shared with the backup destination - the biggest VM/Storage multiplied by the number of tasks
   * when counting tasks for each node assume: `NumberOfExportTasksPerNode + NumberOfStoreTasksPerNode`
9. **Server resource requirements:**
   * **CPU**: Assume 0.5 CPU per task, minimum 2 cores - rounded up to full core count supported by the hypervisor or physical server \(i.e. it may be required to round up 2.5 cores to 4 vCPUs if the hypervisor on which the node is deployed doesn't allow 3 vCPUs to be assigned\)
   * **Memory**: 256 MB RAM per task, with a minimum of 6GB
   * when counting tasks assume:
     `TotalNumberOfExportTasks + TotalNumberOfStoreTasks`
10. Finally, if the resulting node count is too big:
    * divide your VMs into multiple backup policies with separate schedules so that some full backups of your VMs will be done on Monday,  some on Tuesday, while the rest will run incremental backups at the same time - this will reduce the value of `TotalSizeOfData` in the previous equations
    * check if the backup window cannot be extended
      * exports usually impact infrastructure more, while store tasks can also safely be done during the day 
      * vProtect will start backups only within the backup window, but once the tasks are started, they may continue even after your backup window ends

## Notes on sizing using different setups

### **Disk-attachment methods**

* read data from locally attached drives \(which may use LAN or SAN behind the scenes depending on your virtualization platform setup\) and write it to the staging space \(local or remote\)
* run a read test from one device to the staging storage to estimate the processing rate
* this method requires usually 1 node per cluster, so treat each cluster separately
* this method also requires time for attachment/detachment of drives

### **Export storage repository methods**

* export data from a specific host to the staging space of vProtect via NFS
* run a test export on any VM to estimate the export speed in your environment
* export methods also usually have limitations on the hypervisor side, so OVM can process only 1 export job simultaneously using a specific set of storage repositories \(on which the VM disks reside, and to which the VM is being exported\), which may impact overall performance - please consult your hypervisor documentation to check for export process limitations
* it is common to share a backup destination with staging space from an external backup provider via NFS \(not from each node\) so that exports are done directly to the backup destination storage

### Direct export from hypervisors or underlying storage

* if you can enable the netcat in SSH Transfer methods, it should result in 2-3 times faster transfer rates compared to standard SSH
* export tasks run against stand-alone hypervisors will be automatically balanced, while those managed by hypervisor managers will be subject to global and per source limits
  * this means that if you configure a maximum number of export tasks per source to 5 and the global number to 10, you will have no more than 5 export tasks running against a single manager regardless of the number of hosts
* when using Ceph RBD in KVM stand-alone or the OpenStack SSH Transfer method, the actual transfer is done directly from Ceph monitors, and this is the network path that needs to be checked when estimating bandwidth - use `rbd export` or mount a test volume over RBD-NBD to test it


**Backup destination**

* if you plan to use common storage for the staging space and backup destination, your reads from the source will be limited by the write speed of your backup destination 
* make sure you have the appropriate bandwidth between the nodes and the backup destination
* verify if the backup destination is able to process IOPS coming from multiple sources - it is common to assume the export rate as the minimum required store rate