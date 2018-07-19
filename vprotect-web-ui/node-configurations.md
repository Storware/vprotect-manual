# Node configurations

Node configurations are groups of settings assignable to set of nodes. So that you don't have to change them on every node separately.

## Available settings

### General

* `Name` - unique name identifying configuration
* `Set as default` - set default configuration to be assigned to new nodes
* `Export path (staging space)` - staging path \(must be owned by `vprotect` user\)
* `Max. allowed backup timestamp difference (for correlation between local DB and backup destination) [s]` - maximum time difference between timestamp of a backup in vProtect and backup destination to match local version of backup with remote one
* `Restore path for Import tasks` - in rare cases you may want to restore backups to custom location within the node before the import process begins
* `Restore path for Mount tasks` - in rare cases you may want to restore backups to custom location within the node before the mount process begins
* `Min. free space for export (staging space threshold) [%]` - amount of storage space left \(percentage\) to force node to wait with starting another export tasks \(node will resume processing when available space will be more than this threshold\)

### Task

* `Maximum no. of index threads` - max. no. of index tasks per node
* `Maximum no. of export threads` - max. no. of export tasks per node \(total\)
* `Maximum export threads per source (HV or HVM)` - max. no. of export tasks per node \(per HV/HVM\)
* `Maximum no. of store threads` - max. no. of store tasks per node
* `Maximum no. of restore threads` - max. no. of restore tasks per node
* `Index timeout (start window length) [min]` - default length of start window for index tasks
* `Export task timeout (start window length) [min]` - default length of start window for export tasks
* `Store task timeout (start window length) [min]` - default length of start window for store tasks
* `Restore task timeout (start window length) [min]` - default length of start window for restore tasks
* `Import task timeout (start window length) [min]` - default length of start window for import tasks
* `Mount task timeout (start window length) [min]` - default length of start window for mount tasks
* `Unmount task timeout (start window length) [min]` - default length of start window for unmount tasks
* `Old backups removal task timeout (start window length) [min]` - default length of start window for old backups removal tasks

### Hypervisor

#### Red Hat Virtualization/oVirt

* `VM snapshot creation timeout [s]` - timeout for snapshot creation operation
* `VM snapshot clone timeout [s]` - timeout for snapshot clone operation
* `VM export timeout [s]` - timeout for export operation
* `Enable multi-DC export` - enables export to separate subdirectories for each DC
* `Data Center to Storage Domain mappings (RHV 3.5.x only)` - \(optional\) this is for manual definition of mapping between Export Storage Domains and Data Centers

#### Oracle VM

* `Pool to Repository mappings` - defines which `Storage Repository ID` must be used to to export backups for `Server Pool` 
* `Job status polling interval [s]` - defines how often progress of a job submitted to OVM manager must be checked

#### Citrix XenServer

* `Export refresh rate [s]` - defines how often progress of a job submitted to Citrix Xen Server must be checked
* `Enable XVA compression` - enables compression for XVA \(full backups\)

#### Proxmox

* `Enable compression` - specifies if backups should be exported in compressed format - if enabled, you can choose between GZIP and LZO formats
* `Backup storage` - name of the storage used to export backups

#### KVM/Xen \(libvirt\)

* `SSH command timeout [s]` - timeout for SSH command that is invoked
* `SSH known hosts file path` - path to the `known_hosts` file \(accessible for `vprotect` user\)
* `LVM snapshot size` - value for “-l” parameter in `lvcreate` command
* `LVM snapshot extents` – value for “-L” parameter in `lvcreate` command
* `SSH progress refresh rate (every 1/x of data size)` - how often to refresh progress when transferring data over SSH \(x times per data size\)

### Backup destinations

This section is used to add/remove backup destinations to the nodes using this configuration. Only backup destinations enabled here can be used by the nodes.

