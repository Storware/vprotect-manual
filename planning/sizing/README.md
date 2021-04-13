# Sizing Guide

There are two types of people: those who are doing backups, and those who will be doing them. But just **doing** backups is not everything. To avoid unnecessary surprises the best strategy is to **plan** your backup environment/procedure **before implementing** it. In this chapter, we have collected generic hints and guides which you might find useful while thinking about your **vProtect** implementation.

1. Collect information about the $$TotalSizeOfData$$ to be protected in your environment
   * this is the size of your VMs/Storages that will be transferred within a backup window
     * for general sizing assume all backups to be full
   * if your staging space is separate from the backup destination - please also check what are the biggest VMs/Storages that may end up in your staging area 
2. Assume $$BackupWindow$$ length - backups are usually executed overnight, so 10h-12h is a common practice
3. Run **test transfer** some test file to estimate maximum achievable bandwidth per thread  \($$SingleThreadTransfer$$\) - from the hypervisor \(or manager\) to the node
   * recommended 10 simultaneous transfers and result divided by 10 thread - to see if other limitations of the environment do not impact the total transfer rate \(one of such common limitations is disk read performance in the virtualization platform\)
   * usually, all methods use snapshots to do backup - please check if snapshot removal in your environment doesn't take a significant amount of time, as it is a highly resource-intensive operation that impacts overall backup time - especially when running multiple export jobs in parallel
4. Estimate **the** **number of the nodes**
   * required bandwidth per node:

     $$RequiredBandwidth=TotalSizeOfData / BackupWindow$$

   * the total number of export tasks \(note, that usually, other aspects such as snapshot handling, file system scanning during export, and infrastructure bottlenecks when using multiple threads will impact the overall speed\):

     $$TotalNumberOfExportTasks = RequiredBandwidth/(70\%*SingleThreadTransferSpeed$$

   * the number of the nodes:

     $$NumberOfTheNodes=TotalNumberOfExportTasks/10$$ **\(10 is recommended maximum number of export tasks per Node\)**

   * notice that the $$TotalSizeOfData$$ doesn't mean that it is a full backup only, as you can mix full and incremental backups
   * granularity is a single hypervisor or storage provider, so at maximum - you cannot have more nodes than hypervisors storage providers in your environment
   * if you have multiple clusters and you want to use the disk-attachment method, this automatically implies a minimum of 1 node per cluster
5. Estimate the total **store rate** in the backup destination
   * if multiple nodes were required, sum total amount of data from all nodes
   * if your backup destination is accessible over LAN
     * do a test transfer from the node to the backup destination to verify if the performance on the backup destination is able to receive such load
6. $$NumberOfExportTasksPerNode$$
   * we recommend to use the same node configuration for multiple nodes, so the same limit value will be applied to all nodes sharing configuration \(except when using the "disk attachment" backup strategy - then we recommend using a separate configuration for each node\)
   * this implies, that we recommend assuming this value as follows \(rounded down\):

     $$NumberOfExportTasksPerNode=TotalNumberOfExportTasks/NumberOfTheNodes$$
7. $$NumberOfStoreTasksPerNode$$ usually depends on the backup destination performance
   * recommended is the value equal to the $$NumberOfExportTasksPerNode$$ or higher
   * reduce this value only when your backup provider starts to have significant I/O latency which eventually leads to a slower write rate than with the fewer number of threads - this will typically result in higher staging space occupation as backups will be kept for a longer period of time in temporary space
8. **Node resource requirements:**
   * **CPU**: Assume 0,5 CPU per task, minimum 2 cores - rounded up to full core count supported by the hypervisor or physical server \(i.e. it may be required to round 2,5 cores to 4 vCPUs if hypervisor on which node is deployed doesn't allow to assign 3 vCPUs\)
     * if SSH transfer \(without netcat\) or client-side deduplication is used \(i.e. VDO\) assume 1 CPU per task
   * **Memory**: 256 MB RAM per task., minimum of 2GB
   * **Staging space:** if not shared with backup destination - biggest VM/Storage multiplied by the number of tasks
   * when counting tasks for each node assume:$$NumberOfExportTasksPerNode+NumberOfStoreTasksPerNode$$ 
9. **Server resource requirements:**
   * **CPU**: Assume 0,5 CPU per task, minimum 2 cores - rounded up to full core count supported by the hypervisor or physical server \(i.e. it may be required to round 2,5 cores to 4 vCPUs if hypervisor on which node is deployed doesn't allow to assign 3 vCPUs\)
   * **Memory**: 256 MB RAM per task., minimum of 6GB
   * when counting tasks assume:

     $$TotalNumberOfExportTasks+TotalNumberOfStoreTasks$$
10. Finally, if the resulting node count is too big:
    * divide your VMs into multiple backup policies with separate schedules, such as some full backups of your VMs will be done on Monday,  some on Tuesday, while the rest will run incremental backups at the same time - this will reduce the value of $$TotalSizeOfData$$ in the previous equations
    * check if the backup window cannot be extended
      * exports usually impact infrastructure more, while store tasks can safely be done also during the day 
      * vProtect will start backups only within the backup window, but once the tasks are started, they may continue even after your backup window ends\)

## Notes on sizing using different setups

### **Disk-attachment methods**

* reads data from locally attached drives \(which may use LAN or SAN behind the scenes depending on your virtualization platform setup\) and write it to staging space \(local or remote\)
* run test read from one device to the staging storage to estimate processing rate
* this method requires usually 1 node per cluster, so treat each cluster separately
* this method also requires time for attachment/detachment of drives

### **Export storage repository methods**

* export data from the specific host to the staging space of the vProtect over NFS
* run a test export on any VM to estimate export speed in your environment
* export methods usually have also limitations on the hypervisor side, such as OVM can process simultaneously only 1 export job using a specific set of storage repositories \(on which VM disks reside, and to which VM is being exported\), which may impact overall performance - please consult the documentation of your hypervisor to check limitations of export process limitations
* it is common to share a backup destination with staging space from an external backup provider over NFS \(not from each node\) so that exports are done directly to the backup destination storage

### Direct export from hypervisors or underlying storage

* if you can enable the netcat in SSH Transfer methods, it should result in 2-3x times faster transfer rates compared to standard SSH
* export tasks run against stand-alone hypervisors will be automatically balanced while managed by hypervisor managers will be subject to the global and per source limits
  * which means that if you configure a maximum number of export tasks per source to 5 and global to 10- you will have no more than 5 export tasks running against a single manager regardless of the number of hosts
* when using Ceph RBD in KVM stand-alone or OpenStack SSH Transfer method - actual transfer is done directly from Ceph monitors, and this is the network path that needs to be checked when estimating bandwidth - use `rbd export` or mount a test volume over RBD-NBD to test it

**Backup destination**

* if you plan to use common storage for staging space and backup destination your reads from the source will be limited by the write speed of your backup destination 
* make sure to have appropriate bandwidth between nodes and backup destination
* verify if the backup destination is able to process IOPS coming from multiple sources - it is common to assume export rate as a minimum required store rate

