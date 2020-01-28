# VMware vSphere/ESXi setup

1. For obtaining license for VMware vSphere/ESXi backup feature contact us at [info@storware.eu](mailto:info@storware.eu)
2. Backup user must have following privileges for backup/restore procedure:
   * VirtualMachine &gt; Change Configuration
   * VirtualMachine &gt; Edit Inventory
   * VirtualMachine &gt; Provisioning
   * VirtualMachine &gt; Snapshot management
   * Datastore &gt; Browse datastore
   * Datastore &gt; Update virtual machine files
   * Datastore &gt; Update virtual machine metadata
   * Resource &gt; Assign virtual machine to resource pool
   * Tasks &gt; Create task
3. Add your vCenter as Hypervisor Manager in Web UI - `https://vcenter.hostname.or.ip`
   * or if you have stand-alone ESXi hosts - add all hypervisors separately in Hypervisors tab - just their hostnames or IPs and credentials

