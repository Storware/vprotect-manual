# Glossary of terms

* **Backup Desintation** - backup provider or storage space holding backups on vProtect node where backup files are copied to from **Staging Space** - default and the only currently supported is PowerProtect DD via Boost FS
* **Backup SLAs** - are responsible for automation of backups of Virtual Environments or Storage instances. Backup SLA consist of the **policy** and **schedule**.
* **Cluster** - corresponds to server pools/clusters/availability zones that have been detected during inventory synchronization of **Hypervisors**
* **Hypervisors** - a list of hypervisors automatically discovered during inventory synchronization of **Hypervisor Manager** or manually added to vProtect
* **Hypervisor Managers** - a list of hypervisors managers added to vProtect.
* **Instances** - a list of currently known virtual machines/storage.
* **Inventory synchronization** - a task which index the contents of **Hypervisor Manager**, **Storage Provider** or **Hypervisor** (if it's not managed by Hypervisor Manager)
* **Mounted backups** - a list of backups which has been mounted by vProtect and can be browsed.
* **Node** - machine or VM with installed vProtect node, its main job is to execute backup, restore and mount tasks. It should have access to **backup destination** and **staging space**.
* **Node Configuration** - contains settings for nodes to describe their behaviour during tasks execution such as maximum numbers of simultaneous backup tasks, timeouts or **backup destinations** which can be used by nodes. One node configuration can be attached to many **nodes**.
* **Policy** - allow you to group virtual machines or storage instances. Each policy can have multiple **schedules** assigned.
* **Recovery Plans** - are used to automate DR process, so that vProtect executes multiple restore operations to the target environment with preconfigured settings.
* **Schedule** - allow you to invoke specific policies periodically. This allows you to back up multiple VMs or storage instances automatically.
* **Snapshot SLAs** - are responsible for automation of creating snapshots of Virtual Environments or storage instances. Backup SLA consist of the **policy** and **schedule**. Instance has to be assigned to **snapshot policy** in order to execute snapshot on demand.
* **Staging Space** - temporary space for backup files mounted on a vProtect Node; ddefault and recommended configuration is shared space on a PowerProtect DD via Boost FS.
* **Storage** - corresponds to datastores/storage repositories/storage domains that have been detected during inventory synchronization of **Hypervisors**.
* **Storage Provider** - software storage platform that provides storage instances