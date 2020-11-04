# Nodes

## Nodes Instances

Node instances are a list of vProtect nodes currently existing in our environment. You can easily check basic information about nodes or change the assigned node configuration.

![](../.gitbook/assets/nodes%20%281%29.jpg)

## Nodes Configurations

#### You can also perform the same action thanks to the CLI interface: [CLI Reference](cli-reference.md#node-configurations)

Node configurations are groups of settings assignable to a set of nodes. So that you don't have to change them on every node separately.

![](../.gitbook/assets/nodes-configurations.jpg)

## Available settings

### General

* `Name` - unique name identifying configuration
* `Set as default` - set default configuration to be assigned to new nodes

**Storage**

* `Export path (staging space)` - staging path \(must be owned by `vprotect` user\)
* `Restore path for import tasks` - in rare cases, you may want to restore backups to a custom location within the node before the import process begins
* `Restore path for mount tasks` - in rare cases, you may want to restore backups to a custom location within the node before the mounting process begins
* `Min. free space for export [%]` - amount of storage space left to force a node to wait with starting another export tasks

**Backup Process**

* `Max. allowed backup timestamp difference [s]`- maximum time difference between the timestamp of a backup in vProtect and backup destination to match the local version of backup with the remote one 
* `Minimal free space for snapshot [GB]` - amount of storage space left to force a node to wait with starting another task
* `Minimal free space for snapshot [%]` - amount of storage space left to force a node to wait with starting another task
* `Dynamically attached disks slot offset` - this setting forces a shift of a disk slot number that node reads/writes from when the disk-attachment method is used - currently used in the Nutanix disk-attachment method when you have block devices not reported by hypervisors API, such as iSCSI mounted block devices. When set to 0, vProtect will mount drives just after the last occupied \(and reported by hypervisor API\) slot \(which means that block device number 3 in API will be /dev/sdc in OS\). In general, N means that vProtect will shift N slots, so 1 will make 3rd device be treated as 4th in OS /dev/sdd\)
* `Netcat min. port` - min. Netcat port range
* `Netcat max. port` - max. Netcat port range
* `Number of netcat transfer attempts` - maximum number of attempts

![](../.gitbook/assets/nodes-general%20%281%29.jpg)

### Task

![](../.gitbook/assets/nodes-task.jpg)

**INVENTORY SYNCHRONIZATION**

* `Inventory synchronization timeout (start window length) [min]` - default length of the start window for index tasks

**BACKUP**

* `Maximum number of export threads` - max. number of export tasks per node \(total\)
* `Maximum export threads per source (HV or HVM)` - max. number of export tasks per node and per HV/HVM
* `Export task timeout (start window length) [min]` - default length of the start window for export tasks
* `Maximum number of store threads` - max. number of store tasks per node
* `Store task timeout (start window length) [min]` - default length of the start window for store tasks
* `Old backups removal task timeout (start window length) [min]` - default length of start window for old backups removal tasks

**RESTORE**

* `Maximum number of restore threads` - max. number of restore tasks per node \(total\)
* `Restore task timeout (start window length) [min]` - default length of start window for restore tasks
* `Maximum number of import threads` - max. number of import tasks per node \(total\)
* `Import task timeout (start window length) [min]` - default length of the start window for import tasks

**SNAPSHOT MANAGEMENT**

* `Old snapshot removal task timeout (start window length) [min]` - default length of start window for old snapshot removal tasks
* `Snapshot reversion task timeout (start window length) [min]` - default length of start window for snapshot reversion tasks

**FILE-LEVEL RESTORE \(MOUNTED BACKUPS\)**

* `Mount task timeout (start window length) [min]` - default length of start window for mount tasks
* `Unmount task timeout (start window length) [min]` - default length of the start window for unmount tasks

### Backup destinations

This section is used to add/remove backup destinations to the nodes using this configuration. Only backup destinations enabled here can be used by the nodes.

![](../.gitbook/assets/nodes-backup-destinations%20%281%29.jpg)

