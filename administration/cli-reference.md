# CLI Reference

## Overview

Every node provides CLI that can be used to manage configuration and invoke tasks on vProtect Node. All of the commands are executed using `vprotect` command. The general syntax is as shown below:

```text
[root@vProtect3 ~]# vprotect
usage: vprotect <COMMAND> -<ARG_1> ... -<ARG_N>
COMMAND is one of the following:
 node                   Node management
 config                 Node configuration management
 hv                     Hypervisor management
 hvm                    Hypervisor manager management
 hc                     Hypervisor cluster management
 hs                     Hypervisor storage management
 vm                     Virtual machine management
 vmpolicy               VM backup policy management
 bd                     Backup destination management
 sched                  Schedule management
 brvm                   VM backup & restore
 brapp                  Application backup & restore
 mnt                    Mounted backup management
 task                   Task management
 login                  User login
 logout                 Node and user logout
 stop                   Safely stops node
 snappolicy             Snapshot policy management
 app                    Application backup management
 appconf                App command execution management
 apppolicy              Application backup policy management
 restorejob             Restore jobs
 recplan                Recovery plan policies
 status                 Shows node status
 start                  Starts node
```

## Starting and stopping node

* To check if vProtect Node is running, run:

  ```text
  systemctl status vprotect-node
  ```

  or

  ```text
  vprotect status
  ```

* To start vProtect Node run:

  ```text
  systemctl start vprotect-node
  ```

  or

  ```text
  vprotect start
  ```

* To safely stop vProtect Node \(this command waits for all tasks to be cancelled, so that temporary objects are cleaned up\) run:

  ```text
  systemctl stop vprotect-node
  ```

  or

  ```text
  vprotect stop
  ```

* In emergency cases you may need to kill the engine without task clean up - get PID of the node by running:

  ```text
  vprotect status
  ```

  and kill process using:

  ```text
  kill <PID>
  ```

## User login

* To log in to CLI use the following command:

  ```text
  vprotect login -u USER_NAME
  ```

where `USER_NAME` is your admin account in vProtect. You will be prompted for password. From this moment you are able execute commands.

* Once you completed working with CLI, you can log out by running:

  ```text
  vprotect logout -u
  ```

## Note about GUIDs

All of the commands require GUID of the object that you want to refer to in command. GUID is found in the first column of typical list command, e.g.:

1. Let's list VMs and get VM GUID:

   ```text
    [root@vProtect3 ~]# vprotect vm -l
                    GUID                              Name                   Hypervisor       Present   VM group    Protected       Last backup        
    ------------------------------------  -----------------------------  -------------------  -------  -----------  ---------  ----------------------  
    b2555a74-97bb-44e4-9c68-ed1ac2ddffcd  Windows 7 (32-bit)            xcp-ng.storware.local  true    PM-T1_Group  false      2020-06-03 00:30 (Tue)
   ```

2. Now let's show details of the VM:

   ```text
   [root@vprotect ~]# vprotect vm -g b2555a74-97bb-44e4-9c68-ed1ac2ddffcd
   Property                                      Value                                                                             
   --------------------------------------------  ------------------------------------------------------------  

   GUID                                          b2555a74-97bb-44e4-9c68-ed1ac2ddffcd  
   Name                                          Windows 7 (32-bit)  
   UUID                                          9a486d85-c5c6-868b-50f4-3b27f4e4f968  
   Present                                       true  
   HV type                                       CITRIX  
   HVM type                                      -  
   Inc. backup snapshot                          Windows 7 (32-bit): 2020-06-15 19:10:22.0 (6626e7d9-0900-4bfa-a961-85f5379b48d4)  
   Tags                                          [vPro-Citrix]  
   Hypervisor                                    10.40.0.29 (1048d561-b0f5-4f1d-b558-856b28e0283e)  
   HV manager                                    -  
   Backups                                       29  
   VM backup policies                            Citrix Hypervisor | XCP-ng (1f02821f-75f9-433f-9698-5694f130e1df)  
   Protected                                     true  
   Last backup                                   2020-06-15 19:10 (Mon)  
   Warnings present                              true  
   VM export/import mode                         INHERIT  
   Quiesce/freeze before snapshot                false  
   SSH host                                      -  
   SSH port                                      22  
   SSH user                                      -  
   SSH key path                                  -  
   Pre-snapshot CMD exec. enabled                false  
   Pre-snapshot CMD                              []  
   Post-snapshot CMD exec. enabled               false  
   Post-snapshot CMD                             []  
   Pre-snapshot ignored exit codes               -  
   Pre-snapshot standard error output handling   DONT_IGNORE  
   Post-snapshot ignored exit codes              -  
   Post-snapshot standard error output handling  DONT_IGNORE
   ```

**Note**: that UUID is the ID used by hypervisor or HV manager, while GUID is the ID used by vProtect to uniquely identify objects.

Node GUID can be replaced by `this` keyword to state that action such as restore or mount is going to be run on node where the command is executed from. For instance, to restore backup with given GUID on current node to `/vprotect_data`:

```text
vprotect br -r 2132182d-e9ab-4478-a1db-48222b0e515b this /vprotect_data
```

Similarly, backup destinations can be referred by names. So backup of some VM with given GUID to backup destination with name `ISP` can be done like this:

```text
vprotect br -b b6f96d43-6f55-468f-a7a0-8aa6219fdf4e ISP
```

## Nodes

To list or register node use `vprotect node` sub-command:

```text
[root@vprotect ~]# vprotect node
Required option: [-r Register node, -R Check and Register node, -d Delete node, -e Register existing node, -l List nodes]

usage: node -d <GUID | NAME> | -e <NODE_NAME> <ADMIN_LOGIN> <API_URL> <[PASSWORD]> | -l | -r <NODE_NAME> <ADMIN_LOGIN> <API_URL> <[PASSWORD]> | -R <NODE_NAME>
       <ADMIN_LOGIN> <API_URL> <[PASSWORD]>
Node management
 -d,--delete <GUID | NAME>                                                  Delete node
 -e,--register-existing <NODE_NAME> <ADMIN_LOGIN> <API_URL> <[PASSWORD]>    Register existing node
 -l,--list                                                                  List nodes
 -r,--register <NODE_NAME> <ADMIN_LOGIN> <API_URL> <[PASSWORD]>             Register node
 -R,--check-and-register <NODE_NAME> <ADMIN_LOGIN> <API_URL> <[PASSWORD]>   Check and Register node
```

### Examples

* To list all nodes registered

  ```text
  vprotect node -l
  ```

* Example node registration

  ```text
  vprotect node -r node1 admin https://localhost:8181/api
  ```

* You may need to re-register node if vProtect Server address changes

  ```text
  vprotect node -e node1 admin https://localhost:8181/api
  ```

## **Node configurations**

Use `vprotect config` sub-command add/remove backup destinations from node config, list or show details of node configs.

**Note**: configuration changes, as well as assigning configuration to the nodes can be done _from web UI only_.

```text
[root@vprotect ~]# vprotect config
Required option: [-b Add backup destinations, -r Remove backup destinations, -c Create node config, -s Modify node config., -d Delete node config, -g Show config details, -l List node configs]

usage: config -b <CONFIG_GUID> <[BD_GUID|BD_NAME],...,[BD_GUID|BD_NAME]> | -c <NAME> | -d <GUID> | -g <GUID> | -l | -r <CONFIG_GUID>
       <[BD_GUID|BD_NAME],...,[BD_GUID|BD_NAME]> | -s <GUID> <PROPERTY_NO.> <VALUE | "MAPPING1=VALUE1;MAPPING2=VALUE2">
Node configuration management
 -b,--add-backup-destinations <CONFIG_GUID> <[BD_GUID|BD_NAME],...,[BD_GUID|BD_NAME]>      Add backup destinations
 -c,--create <NAME>                                                                        Create node config
 -d,--delete <GUID>                                                                        Delete node config
 -g,--details <GUID>                                                                       Show config details
 -l,--list                                                                                 List node configs
 -r,--remove-backup-destinations <CONFIG_GUID> <[BD_GUID|BD_NAME],...,[BD_GUID|BD_NAME]>   Remove backup destinations
 -s,--set <GUID> <PROPERTY_NO.> <VALUE | "MAPPING1=VALUE1;MAPPING2=VALUE2">                Modify node config.
```

### **Examples**

* To list all node configurations

  ```text
  vprotect config -l
  ```

* To show details of the given node config \(by GUID\)

  ```text
  vprotect config -g 40212820-880e-11e7-bddf-000c2912fc7b
  ```

* To add backup destination \(second GUID\) to the given node configuration \(first GUID\):

  ```text
  vprotect config -b 40212820-880e-11e7-bddf-000c2912fc7b f27b6018-ce1c-4d67-991e-31d9f4f6662b
  ```

  More backup destinations can be passwd by separating GUIDs with commas.

## Backup destinations

Backup destination management module is used to add, remove backup destination \(i.e. backup provider instance with retention settings\). Backup destination must be assigned the group for scheduler to know where to put backups during automatic backup. For backup on-demand you just need to specify which backup destination should be used.

Backup destinations are assigned to the node configuration. This means that nodes using such configuration are configured to store backups in the selected backup destinations.

**Note** that backup task will fail if it is not able to assign node bacause given BD is not assigned to the node config of the node that is required be be used for given hypervisor.

To manage backup destinations in the system use `vprotect bd` sub-command.

```text
[root@vprotect ~]# vprotect bd
Required option: [-r Reinitialize backup destination, -c Create backup destination (type = ["filesystem", "netbackup", "networker", "s3", "swift", "isp", "azure", "gcs", "avamar"]), -s Modify backup destination, -d Delete backup destination, -D Remove old backups, -g Show backup destination details, -l List backup destinations]

usage: bd -c <NAME> <TYPE> | -d <GUID | NAME> | -D <[GUID | NAME],...,[GUID | NAME]> | -g <GUID | NAME> | -l | -r <[GUID]> | -s <GUID |
       NAME> <PROPERTY_NO.> <VALUE>
Backup destination management
 -c,--create <NAME> <TYPE>                                   Create backup destination (type = ["filesystem", "netbackup", "networker",
                                                             "s3", "swift", "isp", "azure", "gcs", "avamar"])
 -d,--delete <GUID | NAME>                                   Delete backup destination
 -D,--remove-old-backups <[GUID | NAME],...,[GUID | NAME]>   Remove old backups
 -g,--details <GUID | NAME>                                  Show backup destination details
 -l,--list                                                   List backup destinations
 -r,--reinit <[GUID]>                                        Reinitialize backup destination
 -s,--set <GUID | NAME> <PROPERTY_NO.> <VALUE>               Modify backup destination
```

### Setting up parameters for backup destinations

In general `vprotect bd -s <GUID> <PROPERTY_NO.> <VALUE>` command sets value of property with the given number of BD with `GUID`. Property numbers are returned in detailed view of each BD \(fields obviously are different for each backup provider\). After you create backup destination show default values with `-g`. Then use property numbers \(first number in the property/value line of the detailed view\).

Retention time settings are in interpreted in days \(just give number without any additional suffixes\).

There are however some mode/type fields which require string to be typed in correct format:

1. Amazon S3 - `Backup mode` - must be either:
   * `SINGLE_BUCKET` - single bucket for all VMs
   * `ONE_BUCKET_PER_VM` - separate bucket for each VM \(note that default limit is 100 buckets in Amazon S3\)
2. Swift - `Auth method` - authentication method used to authenticate Swift BD:
   * `BASIC`
   * `TEMPAUTH`
   * `KEYSTONE`
3. Time zones must be specified in the format shown in this table: [List of tz database time zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)​

### Examples

* To list all backup destinations

  ```text
  vprotect bd -l
  ```

* To show details of the given HV manager \(by GUID\) - **note** that each BD has different set of fields - numbers at the beginning of a line indicate an indentifier of a field you want to set from CLI:

  ```text
  [root@vprotect ~]# vprotect bd -g 3263b196-056e-4485-9e8f-932798b58eb3
  Property                                       Value                                 
  ---------------------------------------------  ------------------------------------  

  GUID                                           3263b196-056e-4485-9e8f-932798b58eb3  
  Node configs                                   0  
  Type                                           S3  
  Total available space                          -  
  Total used space                               6.3 GiB  
  1. Name                                        AWS vProGuide  
  2. Pre-access CMD exec. enabled                false  
  3. Pre-access CMD                              []  
  4. Post-access CMD exec. enabled               false  
  5. Post-access CMD                             []  
  6. Default                                     false  
  7. Retention (full) - keep last N backups      4  
  8. Retention (full) - keep newer than          99999d   
  9. Retention (inc.) - keep last N backups      30  
  10. Retention (inc.) - keep newer than         30d   
  11. Bucket key                                 -  
  12. Bucket name                                vproguide  
  13. Endpoint URL                               -  
  14. Access key                                 ***  
  15. Secret key                                 ***  
  16. Backup mode                                SINGLE_BUCKET  
  17. Record backup time after store             false  
  18. Encryption enabled                         true  
  19. Resolve host name to IP before connection  false  
  20. Path style access                          false  
  21. Region
  ```

* Remember that to use a BD you need to create a new entry of a given typ and then configure its properties \(example for IBM Spectrum Protect \[ISP\], assume that GUID returned by first command was `bb74ef6c-f6de-4783-bfa7-70fc2376fb08` \):

  ```text
  [root@localhost vprotect]# vprotect bd -c MyTSM isp
  [root@vprotect ~]# vprotect bd -g bb74ef6c-f6de-4783-bfa7-70fc2376fb08
  Property                                   Value                                 
  -----------------------------------------  ------------------------------------  

  GUID                                       bb74ef6c-f6de-4783-bfa7-70fc2376fb08  
  Node configs                               1  
  Type                                       ISP  
  Total available space                      -  
  Total used space                           0 B  
  1. Name                                    IBM Spectrum Protect  
  2. Pre-access CMD exec. enabled            false  
  3. Pre-access CMD                          []  
  4. Post-access CMD exec. enabled           false  
  5. Post-access CMD                         []  
  6. Default                                 false  
  7. Retention (full) - keep last N backups  4  
  8. Retention (full) - keep newer than      30d   
  9. Retention (inc.) - keep last N backups  30  
  10. Retention (inc.) - keep newer than     30d   
  11. dsm.opt file path                      /opt/vprotect/dsm.opt  
  12. Node name                              vprotect  
  13. Node password                          ***  
  14. Time zone                              Europe/Sarajevo  
  15. Backup progress refresh rate           20  
  16. Restore progress refresh rate          20
  ```

  Now set the properties:

  * retention - full - 4 versions / 30 days
  * retention - incremental - 28 versions / 30 days
  * ISP node name
  * ISP node password
  * ISP server time zone

  ```text
  vprotect bd -s c1d7fa34-e558-4a1c-af1f-46e88da7bcf5 2 4
  vprotect bd -s c1d7fa34-e558-4a1c-af1f-46e88da7bcf5 3 30
  vprotect bd -s c1d7fa34-e558-4a1c-af1f-46e88da7bcf5 4 28
  vprotect bd -s c1d7fa34-e558-4a1c-af1f-46e88da7bcf5 5 30
  vprotect bd -s c1d7fa34-e558-4a1c-af1f-46e88da7bcf5 7 vprotect
  vprotect bd -s c1d7fa34-e558-4a1c-af1f-46e88da7bcf5 8 vpr0tectPass
  vprotect bd -s c1d7fa34-e558-4a1c-af1f-46e88da7bcf5 9 Europe/Warsaw
  ```

## HV managers \(oVirt/RHV/Oracle VM/Nutanix/Kubernetes\)

Hypervisor manager management module is used to add, remove hypervisor managers \(currently only oVirt and RHV managers\) and invoke indexing task. Indexing tasks gathers information about hypervisors and VMs running in the managed environment and updates their location if the VM has been moved to the different hypervisor.

To manage HV managers in the system use `vprotect hvm` sub-command.

**Note** that if you're using RHV/oVirt/Oracle VM/Nutanix/Kubernetes then hypervisors will be detected automatically as a part of index task. So there is no need to define every hypervisor manually as they will be detected automatically.

```text
[root@vprotect ~]# vprotect hvm
Required option: [-c Create hypervisor manager (type = ["rhev", "rhv", "ovm", "nutanix", "openstack", "vcenter" , "kubernetes", "openshift", "aws"]), -s Index inventory on hypervisor manager, -d Delete hypervisor manager, -e Set export/import mode for HV manager, -u Set user/password or access key/secret key, -V List VMs managed by hypervisor manager, -g Show HV manager details, -sK Set SSH key path, -l List hypervisor managers, -L List hypervisors managed by hypervisor manager, -m Modify hypervisor manager, -n Set node for hypervisor]

usage: hvm -c <URL | ACCOUNT_ID> <TYPE> | -d <GUID | HOST> | -e <GUID | HOST> <DEFAULT|DISK_ATTACHMENT|DISK_IMAGE_TRANSFER|SSH_TRANSFER> | -g <GUID | HOST> |
       -l | -L <GUID | HOST> | -m <GUID | HOST> <URL> | -n <GUID | HOST> <NODE_GUID | NODE_NAME> | -s <GUID | HOST> | -sK <GUID> <SSH_KEYPATH> | -u <GUID |
       HOST> <USER/ACCESS_KEY> <PASSWORD/SECRET_KEY> | -V <GUID | HOST>
Hypervisor manager management
 -c,--create <URL | ACCOUNT_ID> <TYPE>                                                                  Create hypervisor manager (type = ["rhev", "rhv",
                                                                                                        "ovm", "nutanix", "openstack", "vcenter" ,
                                                                                                        "kubernetes", "openshift", "aws"])
 -d,--delete <GUID | HOST>                                                                              Delete hypervisor manager
 -e,--set-export-import-mode <GUID | HOST> <DEFAULT|DISK_ATTACHMENT|DISK_IMAGE_TRANSFER|SSH_TRANSFER>   Set export/import mode for HV manager
 -g,--details <GUID | HOST>                                                                             Show HV manager details
 -l,--list                                                                                              List hypervisor managers
 -L,--list-hvs <GUID | HOST>                                                                            List hypervisors managed by hypervisor manager
 -m,--modify <GUID | HOST> <URL>                                                                        Modify hypervisor manager
 -n,--set-node <GUID | HOST> <NODE_GUID | NODE_NAME>                                                    Set node for hypervisor
 -s,--sync <GUID | HOST>                                                                                Index inventory on hypervisor manager
 -sK,--set-ssh-key-path <GUID> <SSH_KEYPATH>                                                            Set SSH key path
 -u,--credentials <GUID | HOST> <USER/ACCESS_KEY> <PASSWORD/SECRET_KEY>                                 Set user/password or access key/secret key
 -V,--list-vms <GUID | HOST>                                                                            List VMs managed by hypervisor manager
```

### Examples

* To list all hypervisor managers

  ```text
  vprotect hvm -l
  ```

* To show details of the given HV manager \(by GUID\)

  ```text
  vprotect hvm -g 4c999c85-e223-4df3-9c40-7b0115234c8c
  ```

* To index VMs and hypervisors on the given HV manager:

  ```text
  vprotect hvm -i 4c999c85-e223-4df3-9c40-7b0115234c8c
  ```

* To create a HV manager you need to execute the following commands:
  * Create HV manger entry with given type:

    ```text
    vprotect hvm -c https://dovirt-m.lab.local/ovirt-engine/api/v3 rhev
    ```

  * Use GUID returned to set credentials:

    ```text
    vprotect hvm -u 4c999c85-e223-4df3-9c40-7b0115234c8c admin@internal password
    ```

  * By default current node is used for created HV manager - you may change it with this command \(first HV GUID, then node GUID\):

    ```text
    vprotect hvm -n 4c999c85-e223-4df3-9c40-7b0115234c8c e2673e8f-66fc-4e9f-aaef-20958c4c2b01
    ```

    Hypervisors connected to the HV manager will have node of the HV manager assigned by default. For backup export always node assigned to the HV is used.

## Hypervisors

Hypervisor management module is used to add, remove hypervisors and invoke indexing task. Indexing tasks gathers information about VMs running on the hypervisor and updates their location if the VM has been moved within the pool.

To manage hypervisors in the system use `vprotect hv` sub-command.

**Note** that if you're using RHV/oVirt/Oracle VM then hypervisors will be detected automatically as a part of index task. So there is no need to define OVM hypervisors and for RHV/oVirt KVM hosts will be detected automatically.

```text
[root@vprotect ~]# vprotect hv
Required option: [-c Create hypervisor (type = ["citrix", "xen", "kvm", "proxmox", "esxi" , "hyperv"]), -s Synchronize inventory with hypervisor, -d Delete hypervisor, -e Set export/import mode for hypervisor, -u Set user/password, -g Show hypervisor details, -sK Set SSH key path, -l List hypervisors, -L List VMs for hypervisor, -m Modify hypervisor, -n Set node for hypervisor]

usage: hv -c <HOST> <TYPE> | -d <GUID | HOST> | -e <GUID | HOST> <DEFAULT|VM_IMAGE_PLUS_INCREMENTAL_DISKS|CHANGED_BLOCK_TRACKING> | -g <GUID | HOST> | -l | -L
       <GUID | HOST> | -m <GUID | HOST> <NEW_HOST> | -n <GUID | HOST> <NODE_GUID> | -s <GUID | HOST> | -sK <GUID> <SSH_KEYPATH> | -u <GUID | HOST> <USER>
       <PASSWORD>
Hypervisor management
 -c,--create <HOST> <TYPE>                                                                                    Create hypervisor (type = ["citrix", "xen",
                                                                                                              "kvm", "proxmox", "esxi" , "hyperv"])
 -d,--delete <GUID | HOST>                                                                                    Delete hypervisor
 -e,--set-export-import-mode <GUID | HOST> <DEFAULT|VM_IMAGE_PLUS_INCREMENTAL_DISKS|CHANGED_BLOCK_TRACKING>   Set export/import mode for hypervisor
 -g,--details <GUID | HOST>                                                                                   Show hypervisor details
 -l,--list                                                                                                    List hypervisors
 -L,--list-vms <GUID | HOST>                                                                                  List VMs for hypervisor
 -m,--modify <GUID | HOST> <NEW_HOST>                                                                         Modify hypervisor
 -n,--set-node <GUID | HOST> <NODE_GUID>                                                                      Set node for hypervisor
 -s,--sync <GUID | HOST>                                                                                      Synchronize inventory with hypervisor
 -sK,--set-ssh-key-path <GUID> <SSH_KEYPATH>                                                                  Set SSH key path
 -u,--credentials <GUID | HOST> <USER> <PASSWORD>                                                             Set user/password
```

### Examples

* To list all hypervisors

  ```text
  vprotect hv -l
  ```

* To show details of the given hypervisor \(by GUID\)

  ```text
  vprotect hv -g c93140b8-a898-4aff-8eef-645587ca8289
  ```

* To index VMs on the given HV:

  ```text
  vprotect hv -i c93140b8-a898-4aff-8eef-645587ca8289
  ```

* To create a hypervisor you need to execute the following commands:
  * Create HV entry with given type:

    ```text
    vprotect hv -c 1.2.3.4 citrix
    ```

  * Use GUID returned to set credentials:

    ```text
    vprotect hv -u a757c6e8-ece0-467b-b912-dfe393d1e421 root password
    ```

  * By default current node is used for created hypervisor - you may change it with this command \(first HV GUID, then node GUID\):

    ```text
    vprotect hv -n a757c6e8-ece0-467b-b912-dfe393d1e421 e2673e8f-66fc-4e9f-aaef-20958c4c2b01
    ```

## Hypervisor clusters

Hypervisor clusters management module enables you to view and remove clsters detected on RHV/oVirt/Nutanix/OVM/XenServer enviornments.

To view or delete them use `vprotect hc` sub-command.

```text
[root@localhost ~]# vprotect hc
Incorrect syntax: Missing required option: [-d Delete HV cluster, -l List HV clusters]

usage: hc -d <GUID> | -l
Hypervisor cluster management
 -d,--delete <GUID>                                             Delete HV cluster
 -l,--list                                                      List HV clusters
 -sn,--set-name <GUID> <NAME>                                   Change Hypervisor Cluster name
 -sp,--set-storage-provider <GUID> <STORAGE_PROVIDER_GUID>>     Assign Storage Provider to Hypervisor Cluster. No value unassigns Storage Provider
```

* To list all detected clusters:

  ```text
  vprotect hc -l
  ```

* To delete a cluster with GUID `107bc87a-9adf-4d6c-b732-345dd06c59e9`:

  ```text
  vprotect hc -d 107bc87a-9adf-4d6c-b732-345dd06c59e9
  ```

### Examples

* To list all detected clusters:

  ```text
  vprotect hc -l
  ```

* To delete a cluster with GUID `107bc87a-9adf-4d6c-b732-345dd06c59e9`:

  ```text
  vprotect hc -d 107bc87a-9adf-4d6c-b732-345dd06c59e9
  ```

## Hypervisor storage

Hypervisor storage management module enables you to view and remove storage detected on RHV/oVirt/Nutanix/OVM/XenServer enviornments. You can select storage in restore dialog box for XenServer/Nutanix and RHV/oVirt \(v4\) platforms.

To view or delete them use `vprotect hs` sub-command.

```text
[root@localhost ~]# vprotect hs
Incorrect syntax: Missing required option: [-d Delete HV storage, -l List HV storage]

usage: hs -d <GUID> | -l
Hypervisor storage management
 -d,--delete <GUID>   Delete HV storage
 -l,--list            List HV storage
```

### Examples

* To list all detected storage volumes:

  ```text
  vprotect hs -l
  ```

* To delete a storage volume with GUID `6b5aa45a-5436-47cd-82ce-1c4250742323`:

  ```text
  vprotect hs -d 6b5aa45a-5436-47cd-82ce-1c4250742323
  ```

## Virtual machines

Virtual machine management module is used to provide information about VMs that has been detected on hypervisors, report status of last backup of your VMs \(and all backups for a particular VM\) and set priority for operations invoked on VM.

To manage VMs in the system use `vprotect vm` sub-command.

VMs are detected automatically during `Index` task executed on the HV or HV manager.

```text
[root@vprotect ~]# vprotect vm
Required option: [-A Assign VM to the policy, -D List detected VM disks, -xC Set pre-snapshot CMD as semi-colon-separated string, i.e. "cmd;-a;-b", -wb Acknowledge warnings related to the backup, -L List backups of the VM, -xE Set pre-snapshot CMD exec enabled (1) / disabled (0), -rvS Revert snapshot, -cS Create VM snapshot, -se Set handling for pre-snap standard error. Values: DONT_IGNORE, IGNORE_WITH_WARNING, IGNORE_WITHOUT_WARNING, -S List managed VM snapshots, -T List tasks related to the VM, -si Set pre-snap ignored command exit codes e.g. '15, 101-150' or '*', -gb Show backup details, -d Delete VM, -g Show virtual machine details, -XC Set post-snapshot CMD as semi-colon-separated string, i.e. "cmd;-a;-b", -l List VMs, -XE Set post-snapshot CMD exec enabled (1) / disabled (0), -sC Set SSH access credentials, -sE Set handling for post-snap standard error. Values: DONT_IGNORE, IGNORE_WITH_WARNING, IGNORE_WITHOUT_WARNING, -sH Set SSH access host/port, -sI Set post-snap ignored command exit codes e.g. '15, 101-150' or '*', -w Acknowledge all backup warnings related to the VM, -sK Set SSH key path, -rmS Remove old snapshots]

usage: vm -A <GUID> <VM_POLICY_GUID> | -cS <GUID> <RULE_GUID> <PRIORITY> | -D <GUID> | -d <GUID> | -g <<VM_GUID>> | -gb <<BACKUP_GUID>> | -L <GUID> | -l |
       -rmS <VM_GUID,...,VM_GUID> | -rvS <SNAPSHOT_GUID> | -S <GUID> | -sC <GUID> <SSH_USER> <SSH_PASS> | -se <GUID> <HANDLING> | -sE <GUID> <HANDLING> | -sH
       <GUID> <SSH_HOST> <SSH_PORT> | -si <GUID> <IGNORED_EXIT_CODES> | -sI <GUID> <IGNORED_EXIT_CODES> | -sK <GUID> <SSH_KEYPATH> | -T <GUID> | -w <GUID> |
       -wb <BACKUP_GUID> | -xC <GUID> <CMD_STRING> | -XC <GUID> <CMD_STRING> | -xE <GUID> <0|1> | -XE <GUID> <0|1>
Virtual machine management
 -A,--assign-vm-policy <GUID> <VM_POLICY_GUID>              Assign VM to the policy
 -cS,--create-snapshot <GUID> <RULE_GUID> <PRIORITY>        Create VM snapshot
 -D,--list-disks <GUID>                                     List detected VM disks
 -d,--delete <GUID>                                         Delete VM
 -dnp,--delete-non-present <[PROJECT_UUID]>                 Delete non-present Virtual Machines. Project UUID is optional.
 -g,--details <<VM_GUID>>                                   Show virtual machine details
 -gb,--show-backup-details <<BACKUP_GUID>>                  Show backup details
 -L,--list-backups <GUID>                                   List backups of the VM
 -l,--list                                                  List VMs
 -rmS,--remove-snapshot <VM_GUID,...,VM_GUID>               Remove old snapshots
 -rvS,--revert-snapshot <SNAPSHOT_GUID>                     Revert snapshot
 -S,--list-snapshots <GUID>                                 List managed VM snapshots
 -sC,--set-ssh-credentials <GUID> <SSH_USER> <SSH_PASS>     Set SSH access credentials
 -se,--set-pre-std-error-out <GUID> <HANDLING>              Set handling for pre-snap standard error. Values: DONT_IGNORE, IGNORE_WITH_WARNING,
                                                            IGNORE_WITHOUT_WARNING
 -sE,--set-post-std-error-out <GUID> <HANDLING>             Set handling for post-snap standard error. Values: DONT_IGNORE, IGNORE_WITH_WARNING,
                                                            IGNORE_WITHOUT_WARNING
 -sH,--set-ssh-host <GUID> <SSH_HOST> <SSH_PORT>            Set SSH access host/port
 -si,--set-pre-ignored-codes <GUID> <IGNORED_EXIT_CODES>    Set pre-snap ignored command exit codes e.g. '15, 101-150' or '*'
 -sI,--set-post-ignored-codes <GUID> <IGNORED_EXIT_CODES>   Set post-snap ignored command exit codes e.g. '15, 101-150' or '*'
 -sK,--set-ssh-key-path <GUID> <SSH_KEYPATH>                Set SSH key path
 -T,--list-tasks <GUID>                                     List tasks related to the VM
 -w,--ack-all-backup-warnings <GUID>                        Acknowledge all backup warnings related to the VM
 -wb,--ack-backup-warnings <BACKUP_GUID>                    Acknowledge warnings related to the backup
 -xC,--set-pre-snap-cmd <GUID> <CMD_STRING>                 Set pre-snapshot CMD as semi-colon-separated string, i.e. "cmd;-a;-b"
 -XC,--set-post-snap-cmd <GUID> <CMD_STRING>                Set post-snapshot CMD as semi-colon-separated string, i.e. "cmd;-a;-b"
 -xE,--set-pre-snap-cmd-exec-enabled <GUID> <0|1>           Set pre-snapshot CMD exec enabled (1) / disabled (0)
 -XE,--set-post-snap-cmd-exec-enabled <GUID> <0|1>          Set post-snapshot CMD exec enabled (1) / disabled (0)
```

### Examples

* To list all VMs

  ```text
  vprotect vm -l
  ```

* To show details of the given VM \(by GUID\)

  ```text
  vprotect vm -g 0f36f40c-6427-4035-9f2b-1ead6aca3597
  ```

* To add VM \(first GUID\) to the given group \(second GUID\):

  ```text
  vprotect vm -A 0f36f40c-6427-4035-9f2b-1ead6aca3597 3afcd507-a4f5-484d-8d34-53c73d7a5809
  ```

* To show backup history of a VM with given GUID:

  ```text
  vprotect vm -L 0f36f40c-6427-4035-9f2b-1ead6aca3597
  ```

* To create snapshot of a VM with given GUID \(VM must have snapshot policy already assigned\):

  ```text
  vprotect vm -cS 0f36f40c-6427-4035-9f2b-1ead6aca3597
  ```

## VM backup policies

Virtual machine backup policies management module is used to define backup policies of VMs. You can assign different backup priority for a policy when the scheduler invokes backup task. You need first to define a VM policy and then add VMs to it. VM can belong only to a single backup policy.

To manage VM policies in the system use `vprotect vmpolicy` sub-command.

VMs are assigned automatically to the policy only if VM has no policy assigned already. If automatic assignment has been turned on for a policy and either name of the VM matches regular expression, or tag detected \(Citrix/oVirt/RHV/Oracle VM\) matches tag defined for the policy, VM is assigned to the policy, and all schedules for a policy will also be automatically invoked for this VM.

**Note**: it is important to assign backup destination for a policy \(required for node to know where to store backups\)

```text
[root@vprotect ~]# vprotect vmpolicy
Required option: [-rR Remove auto-assignment RE, -rT Remove auto-assignment tag, -aC Set auto-assignment HV clusters, -b Set backup destination for the VM policy, -c Create a new policy, -d Delete a policy, -g Show details, -l List policies, -L List VMs in the policy, -aM Set auto-assignment mode, -m Modify policy, -aN Set auto-removal of non-present VMs flag, -p Set policy's backup task priority (0-100, 50 = default), -aR Add auto-assignment RE, -aT Add auto-assignment tag, -S List schedules for the policy, -s Set schedules for the VM policy, -U Unassign VMs from the policy, -V Assign VMs to the policy]

usage: vmpolicy -aC <GUID > <[HV_CLUSTER_GUID,...,HV_CLUSTER_GUID]> | -aM <GUID> <DISABLED|ASSIGN_ONLY|ASSIGN_AND_REMOVE> | -aN <GUID> <0|1> | -aR <GUID>
       <inc|exc> <REG_EXP> | -aT <GUID> <inc|exc> <TAG> | -b <GUID> <BD_GUID | BD_NAME>> | -c <NAME> | -d <GUID> | -g <GUID> | -l | -L <GUID> | -m <GUID>
       <NAME> | -p <GUID> <PRIORITY> | -rR <GUID> <inc|exc> <REG_EXP> | -rT <GUID> <inc|exc> <TAG> | -S <GUID> | -s <GUID> <SCHED_GUID,...,SCHED_GUID> | -U
       <VM_GUID,...,VM_GUID> | -V <GUID> <VM_GUID,...,VM_GUID>
VM backup policy management
 -aC,--set-auto-assign-hv-clusters <GUID > <[HV_CLUSTER_GUID,...,HV_CLUSTER_GUID]>   Set auto-assignment HV clusters
 -aM,--set-auto-assign-mode <GUID> <DISABLED|ASSIGN_ONLY|ASSIGN_AND_REMOVE>          Set auto-assignment mode
 -aN,--set-auto-remove-non-present <GUID> <0|1>                                      Set auto-removal of non-present VMs flag
 -aR,--add-auto-assign-re <GUID> <inc|exc> <REG_EXP>                                 Add auto-assignment RE
 -aT,--add-auto-assign-tag <GUID> <inc|exc> <TAG>                                    Add auto-assignment tag
 -b,--set-backup-destination <GUID> <BD_GUID | BD_NAME>>                             Set backup destination for the VM policy
 -c,--create <NAME>                                                                  Create a new policy
 -d,--delete <GUID>                                                                  Delete a policy
 -g,--details <GUID>                                                                 Show details
 -l,--list                                                                           List policies
 -L,--list-vms <GUID>                                                                List VMs in the policy
 -m,--modify <GUID> <NAME>                                                           Modify policy
 -p,--set-priority <GUID> <PRIORITY>                                                 Set policy's backup task priority (0-100, 50 = default)
 -rR,--remove-auto-assign-re <GUID> <inc|exc> <REG_EXP>                              Remove auto-assignment RE
 -rT,--remove-auto-assign-tag <GUID> <inc|exc> <TAG>                                 Remove auto-assignment tag
 -S,--list-schedules <GUID>                                                          List schedules for the policy
 -s,--set-schedules <GUID> <SCHED_GUID,...,SCHED_GUID>                               Set schedules for the VM policy
 -U,--unassign-vms <VM_GUID,...,VM_GUID>                                             Unassign VMs from the policy
 -V,--assign-vms <GUID> <VM_GUID,...,VM_GUID>                                        Assign VMs to the policy
```

### Examples

* To list all VM policies

  ```text
  vprotect vmpolicy -l
  ```

* To show details of the given VM \(by GUID\)

  ```text
  vprotect vmpolicy -g 3afcd507-a4f5-484d-8d34-53c73d7a5809
  ```

* To create a new policy

  ```text
  vprotect vmpolicy -c "My Policy"
  ```

* To add `policy1` tag \(include\) for a policy:

  ```text
  vprotect vmpolicy -aT 3afcd507-a4f5-484d-8d34-53c73d7a5809 inc policy1
  ```

* To remove `policy1` tag \(include\) for a policy:

  ```text
  vprotect vmpolicy -rT 3afcd507-a4f5-484d-8d34-53c73d7a5809 inc policy1
  ```

* To set 2 schedules \(GUIDs are comma-separated\) for a policy \(first GUID\):

  ```text
  vprotect vmpolicy -s 3afcd507-a4f5-484d-8d34-53c73d7a5809 391203e3-ad6c-4532-b69c-78b3a5cf4ef5,e4c1c61d-26db-4e41-87cf-2195d5498cde
  ```

* To set backup destination \(you can use name or GUID\) for a policy \(first GUID\):

  ```text
  vprotect vmpolicy -b 3afcd507-a4f5-484d-8d34-53c73d7a5809 ISP
  ```

## Snapshot management policies

Snapshot management policies module enables CDM for your VMs and manages their retention. Currently it is supported for KVM, Citrix, RHV/oVirt/OLVM platforms.

To manage snapshot policies in the system use `vprotect snappolicy` sub-command.

VMs are assigned automatically to the policy only if VM has no policy assigned already. If automatic assignment has been turned on for a policy and either name of the VM matches regular expression, or tag detected \(Citrix/RHV/oVirt/OLVM\) matches tag defined for the policy, VM is assigned to the policy, and all schedules for a policy will also be automatically invoked for this VM.

**Note:** only VMs with assigned snapshot management policy can be snapshot from the CLI or UI

```text
[root@vprotect ~]# vprotect snappolicy
Required option: [-rR Remove rules from policy, -rT Remove auto-assignment tag, -aC Set auto-assignment HV clusters, -c Create a new policy, -d Delete a policy, -g Show details, -l List policies, -L List VMs in the policy, -aM Set auto-assignment mode, -m Modify policy, -aN Set auto-removal of non-present VMs flag, -p Set policy's backup task priority (0-100, 50 = default), -aR Add new rule to selected policy, -r List rules for policy, -aT Add auto-assignment tag, -U Unassign VMs from the policy, -V Assign VMs to the policy]

usage: snappolicy -aC <GUID > <[HV_CLUSTER_GUID,...,HV_CLUSTER_GUID]> | -aM <GUID> <DISABLED|ASSIGN_ONLY|ASSIGN_AND_REMOVE> | -aN <GUID> <0|1> | -aR <NAME>
       <POLICY_GUID> <RETENTION_DAYS> <RETENTION_VERSIONS> | -aT <GUID> <inc|exc> <TAG> | -c <NAME> | -d <GUID> | -g <GUID> | -l | -L <GUID> | -m <GUID>
       <NAME> | -p <GUID> <PRIORITY> | -r <GUID> | -rR <POLICY_GUID> <RULE_GUID,...,RULE_GUID> | -rT <GUID> <inc|exc> <TAG> | -U <VM_GUID,...,VM_GUID> | -V
       <GUID> <VM_GUID,...,VM_GUID>
Snapshot policy management
 -aC,--set-auto-assign-hv-clusters <GUID > <[HV_CLUSTER_GUID,...,HV_CLUSTER_GUID]>   Set auto-assignment HV clusters
 -aM,--set-auto-assign-mode <GUID> <DISABLED|ASSIGN_ONLY|ASSIGN_AND_REMOVE>          Set auto-assignment mode
 -aN,--set-auto-remove-non-present <GUID> <0|1>                                      Set auto-removal of non-present VMs flag
 -aR,--add-rule <NAME> <POLICY_GUID> <RETENTION_DAYS> <RETENTION_VERSIONS>           Add new rule to selected policy
 -aT,--add-auto-assign-tag <GUID> <inc|exc> <TAG>                                    Add auto-assignment tag
 -c,--create <NAME>                                                                  Create a new policy
 -d,--delete <GUID>                                                                  Delete a policy
 -g,--details <GUID>                                                                 Show details
 -l,--list                                                                           List policies
 -L,--list-vms <GUID>                                                                List VMs in the policy
 -m,--modify <GUID> <NAME>                                                           Modify policy
 -p,--set-priority <GUID> <PRIORITY>                                                 Set policy's backup task priority (0-100, 50 = default)
 -r,--list-rules <GUID>                                                              List rules for policy
 -rR,--remove-rules <POLICY_GUID> <RULE_GUID,...,RULE_GUID>                          Remove rules from policy
 -rT,--remove-auto-assign-tag <GUID> <inc|exc> <TAG>                                 Remove auto-assignment tag
 -U,--unassign-vms <VM_GUID,...,VM_GUID>                                             Unassign VMs from the policy
 -V,--assign-vms <GUID> <VM_GUID,...,VM_GUID>                                        Assign VMs to the policy
```

### Examples

* To list all snapshot policies

  ```text
  vprotect snappolicy -l
  ```

* To show details of the given VM \(by GUID\)

  ```text
  vprotect snappolicy -g 3afcd507-a4f5-484d-8d34-53c73d7a5809
  ```

* To create a new policy

  ```text
  vprotect snappolicy -c "My Policy"
  ```

* To add `policy1` tag \(include\) for a policy:

  ```text
  vprotect snappolicy -aT 3afcd507-a4f5-484d-8d34-53c73d7a5809 inc policy1
  ```

* To remove `policy1` tag \(include\) for a policy:

  ```text
  vprotect snappolicy -rT 3afcd507-a4f5-484d-8d34-53c73d7a5809 inc policy1
  ```

* To set 2 schedules \(GUIDs are comma-separated\) for a policy \(first GUID\):

  ```text
  vprotect snappolicy -s 3afcd507-a4f5-484d-8d34-53c73d7a5809 391203e3-ad6c-4532-b69c-78b3a5c
  ```

## Schedules

Schedule management module is used to create schedules associated with VMs or groups of VMs. Each schedule defines days of week and the hour when the backup tasks should be invoked. Each schedule also defines time window in which the backup process must start. If the task queue is full and the tasks starts after the specified amount of time from the schedule – it will be cancelled and marked as failed.

**Note**:

* tasks in the queue are run in the order from the highest to the lowest priority.
* you can assign multiple schedules if you need backup to be executed at different hours on different days
* remember always to assign schedule to VM or VM group – schedule can be assigned only to a single VM or VM group

To manage schedules in the system use `vprotect sched` sub-command.

```text
[root@vprotect ~]# vprotect sched
Required option: [-a Set schedule to be active (1) / not active (0), -c Create schedule for VM (backup type: FULL / INCREMENTAL), -d Delete schedule, -g Show details, -l List schedules, -m Modify schedule (backup type: FULL / INCREMENTAL)]

usage: sched -a <GUID> <0|1> | -c <NAME> <VM_BACKUP | SNAPSHOT | APP_BACKUP> <BACKUP_TYPE> <TIME | INTERVAL> <START_TIME |
       INTERVAL_START_HOUR-INTERVAL_END_HOUR> <DURATION | INTERVAL_FREQUENCY> <everyday | LIST_OF_DAYS_OF_WEEK> <any | FIRST_IN_MONTH | SECOND_IN_MONTH |
       THIRD_IN_MONTH | FOURTH_IN_MONTH | LAST_IN_MONTH> <any | LIST_OF_MONTHS> | -d <GUID> | -g <GUID> | -l | -m <GUID> <NAME> <TYPE> <TIME | INTERVAL>
       <START_TIME | INTERVAL_START_HOUR-INTERVAL_END_HOUR> <DURATION | INTERVAL_FREQUENCY> <everyday | LIST_OF_DAYS_OF_WEEK> <any | FIRST_IN_MONTH |
       SECOND_IN_MONTH | THIRD_IN_MONTH | FOURTH_IN_MONTH | LAST_IN_MONTH> <any | LIST_OF_MONTHS>
Schedule management
 -a,--set-active <GUID> <0|1>              Set schedule to be active (1) / not active (0)
 -c,--create     <NAME> <VM_BACKUP | SNAPSHOT | APP_BACKUP> <BACKUP_TYPE> <TIME | INTERVAL> <START_TIME | INTERVAL_START_HOUR-INTERVAL_END_HOUR> <DURATION |
                 INTERVAL_FREQUENCY> <everyday | LIST_OF_DAYS_OF_WEEK> <any | FIRST_IN_MONTH | SECOND_IN_MONTH | THIRD_IN_MONTH | FOURTH_IN_MONTH | LAST_IN_MONTH> <any | LIST_OF_MONTHS>
                                           Create schedule for VM (backup type: FULL / INCREMENTAL)
 -d,--delete <GUID>                        Delete schedule
 -g,--show-details <GUID>                  Show details
 -l,--list                                 List schedules
 -m,--modify <GUID> <NAME> <TYPE> <TIME | INTERVAL> <START_TIME | INTERVAL_START_HOUR-INTERVAL_END_HOUR> <DURATION | INTERVAL_FREQUENCY> <everyday |
               LIST_OF_DAYS_OF_WEEK> <any | FIRST_IN_MONTH | SECOND_IN_MONTH | THIRD_IN_MONTH | FOURTH_IN_MONTH | LAST_IN_MONTH> <any | LIST_OF_MONTHS>
                                           Modify schedule (backup type: FULL / INCREMENTAL)
```

### Examples

* To list all schedules

  ```text
  vprotect sched -l
  ```

* To create a full backup schedule with name Schedule1 executed every day at 05:00 with 60 minutes of time window:

  ```text
  vprotect sched –c Schedule1 FULL 05:00 60 everyday
  ```

* To create a incremental backup schedule with name Schedule2 executed every Monday, Wednesday and Friday at 17:00 with 90 minutes of time window:

  ```text
  vprotect sched -c Schedule2 INCREMENTAL 17:00 90 mon,wed,fri
  ```

  **Note** that days of week are given as a single comma-separated string of short \(3-letter\) weekday names

* To create schedules \(full backup\) with name Schedule3 and Schedule4 executed every Tuesday at 02:00 and every Wednesday at 03:00 with 90 minutes of time window and assign them to the VM group with ID 123:

  ```text
  vprotect sched –c Schedule3 FULL 02:00 90 tue
  vprotect sched –c Schedule4 FULL 03:00 90 wed
  vprotect vmg -s 3afcd507-a4f5-484d-8d34-53c73d7a5809 8d8649cf-d3a1-4e54-b58b-69e51b2456e2,e694dba7-0fdf-44dc-beec-3c9e480b0621
  ```

  assuming GUIDs generated for new schedules `8d8649cf-d3a1-4e54-b58b-69e51b2456e2` and `e694dba7-0fdf-44dc-beec-3c9e480b0621` respectively:

* To disable a schedule with GUID `6651787d-9a55-421d-8158-ead80a70a9cb`:

  ```text
  vprotect sched -a 6651787d-9a55-421d-8158-ead80a70a9cb 0
  ```

## VM backup/restore

This module is used to manage backup and restore process. It is also used to list backups of a particular VM.

To invoke backup and restore tasks use `vprotect brvm` sub-command.

```text
[root@vprotect ~]# vprotect brvm
Required option: [-b Backup (full), -B Backup (full) with task priority (0-100, 50 = default), -gL Show file details, -F List file systems, -H Restore the backup to the hypervisor. For KVM mixed volumes please provide storages in form of BF_GUID=STORAGE_ID with files separated by semicolon, i.e. "BF_GUID=STORAGE_ID; BF_GUID2=STORAGE_ID2 ...", -i Backup VM (incremental), -I Backup VM (incremental) with task priority (0-100, 50 = default), -l List backups, -L List backup files, -M Restore the backup to the hypervisor manager, -r Restore the backup, -T List tasks related to the backup, -gb Show backup details]

usage: brvm -b <GUID> <BP_GUID | BP_NAME> | -B <GUID> <BP_GUID | BP_NAME> <PRIORITY> | -F <GUID> | -gb <BACKUP_GUID> | -gL <BACKUP_FILE_GUID> | -H <GUID>
       <HV_GUID | HV_HOST> <STORAGE_ID> | -i <VM_GUID> <BP_GUID | BP_NAME> | -I <VM_GUID> <BP_GUID | BP_NAME> <PRIORITY> | -l | -L <GUID> | -M <GUID>
       <HVM_GUID | HVM_HOST> <STORAGE_ID> <[CLUSTER_ID]> | -r <GUID> <NODE_GUID | NODE_NAME> <DIRECTORY> | -T <GUID>
VM backup & restore
 -b,--backup <GUID> <BP_GUID | BP_NAME>                                         Backup (full)
 -B,--backup-with-priority <GUID> <BP_GUID | BP_NAME> <PRIORITY>                Backup (full) with task priority (0-100, 50 = default)
 -F,--list-file-systems <GUID>                                                  List file systems
 -gb,--show-backup-details <BACKUP_GUID>                                        Show backup details
 -gL,--show-files-details <BACKUP_FILE_GUID>                                    Show file details
 -gsi,--show-backup-status-info <BACKUP_GUID>                                   Show backup status info
 -H,--restore-to-hv <GUID> <HV_GUID | HV_HOST> <STORAGE_ID>                     Restore the backup to the hypervisor. For KVM mixed volumes please provide
                                                                                storages in form of BF_GUID=STORAGE_ID with files separated by semicolon, i.e. "BF_GUID=STORAGE_ID; BF_GUID2=STORAGE_ID2 ..."
 -i,--backup-inc <VM_GUID> <BP_GUID | BP_NAME>                                  Backup VM (incremental)
 -I,--backup-inc-with-priority <VM_GUID> <BP_GUID | BP_NAME> <PRIORITY>         Backup VM (incremental) with task priority (0-100, 50 = default)
 -l,--list                                                                      List backups
 -L,--list-files <GUID>                                                         List backup files
 -M,--restore-to-hvm <GUID> <HVM_GUID | HVM_HOST> <STORAGE_ID> <[CLUSTER_ID]>   Restore the backup to the hypervisor manager
 -r,--restore <GUID> <NODE_GUID | NODE_NAME> <DIRECTORY>                        Restore the backup
 -T,--list-tasks <GUID>                                                         List tasks related to the backup
```

### Examples

* To list all backups and their status:

  ```text
  vprotect brvm -l
  ```

* To show backups of a particular VM:

  ```text
  vprotect vm -L 0f36f40c-6427-4035-9f2b-1ead6aca3597
  ```

* To show files that a specific backup consists of:

  ```text
  vprotect brvm -L 4f1c7907-72e9-4797-8470-3f1fbb081751
  ```

* To show detected file systems in specific backup:

  ```text
  vprotect brvm -F 4f1c7907-72e9-4797-8470-3f1fbb081751
  ```

* To create a full backup of a VM and store it in backup destination called `MyTSM`

  ```text
  vprotect brvm -b 0f36f40c-6427-4035-9f2b-1ead6aca3597 MyTSM
  ```

* To restore a full backup with given GUID on the current node \(`this`\) to the `/vprotect_data`

  ```text
  vprotect brvm -r 2132182d-e9ab-4478-a1db-48222b0e515b this /vprotect_data
  ```

* To create an incremental backup \(Citrix only\) of a VM and store it in backup destination called `MyTSM`

  ```text
  vprotect brvm -i 0f36f40c-6427-4035-9f2b-1ead6aca3597 MyTSM
  ```

* To restore VM backup \(first GUID\) to the hypervisor \(second GUID - Citrix XenServer only\) and specific storage

  ```text
  vprotect brvm -H 2132182d-e9ab-4478-a1db-48222b0e515b c93140b8-a898-4aff-8eef-645587ca8289 "Local storage"
  ```

## Mounted backups

Mounted backup management module is used to mount and unmounts backups on the given node. This feature is currently supported for RHV/oVirt/OVM VMs. Each mounted backup can be mounted automatically \(auto-detection of mount points within single root or manually with separate mount points for each volume.

To invoke mount/unmount tasks use `vprotect mnt` sub-command.

```text
[root@vprotect ~]# vprotect mnt
Required option: [-T List tasks related to the mounted backup, -u Unmount previously mounted backup, -F List file systems, -l List mounted backups, -L List mounted files, -mi Mount iSCSI., -m Mount backup according to the MOUNT_SPECIFICATION, -Li List files for selected backup, that can be mounted using iSCSI mode.]

usage: mnt -F <GUID> | -l | -L <GUID> | -Li <BACKUP_GUID> | -m <GUID> <NODE_GUID | NODE_NAME> <auto|manual> <MOUNT_SPECIFICATION> | -mi <BACKUP_GUID>
       <NODE_GUID | NODE_NAME> <ALLOWED_CLIENT,...,ALLOWED_CLIENT> <DISK_GUID,...,DISK_GUID> | -T <GUID> | -u <GUID>
Mounted backup management
 -F,--list-file-systems <GUID>                                                                                           List file systems
 -l,--list                                                                                                               List mounted backups
 -L,--list-files <GUID>                                                                                                  List mounted files
 -Li,--list-iscsi-mountable-files <BACKUP_GUID>                                                                          List files for selected backup, that can be mounted using iSCSI mode.
 -m,--mount <GUID> <NODE_GUID | NODE_NAME> <auto|manual> <MOUNT_SPECIFICATION>                                           Mount backup according to the MOUNT_SPECIFICATION
 -mi,--mount-iscsi <BACKUP_GUID> <NODE_GUID | NODE_NAME> <ALLOWED_CLIENT,...,ALLOWED_CLIENT> <DISK_GUID,...,DISK_GUID>   Mount iSCSI.
 -T,--list-tasks <GUID>                                                                                                  List tasks related to the mounted backup
 -u,--unmount <GUID>                                                                                                     Unmount previously mounted backup
```

### Examples

* To list all mounted backups:

  ```text
  vprotect mnt -l
  ```

* To list all mounted files used by mounted backup with GUID `1ac068d3-4848-4c98-b30b-54ce050b6a95` \(**note** that this is mounted backup GUID not a backup GUID\):

  ```text
  vprotect mnt -L 1ac068d3-4848-4c98-b30b-54ce050b6a95
  ```

* To show detected file systems in specific backup:

  ```text
  vprotect mnt -F 1ac068d3-4848-4c98-b30b-54ce050b6a95
  ```

* To mount all file systems in backup with GUID `2132182d-e9ab-4478-a1db-48222b0e515b` to /mnt/myVM/2017-01-01:

  ```text
  vprotect mnt -m 2132182d-e9ab-4478-a1db-48222b0e515b auto /mnt/myVM/2017-01-01
  ```

* To mount manually file systems in backup with GUID `2132182d-e9ab-4478-a1db-48222b0e515b` with specifying mount points you need to provide semicolon-separated list where you provide name of the volume=mount point \(white space before or after semicolon or equal sign is not allowed\)

  ```text
  vprotect mnt -m 2132182d-e9ab-4478-a1db-48222b0e515b manual "/dev/sda1=/mnt/myVM/sda1;/dev/vg_sda/lv_root=/mnt/myVM/lv_root"
  ```

* To unmount mounted backup with GUID `1ac068d3-4848-4c98-b30b-54ce050b6a95`:

  ```text
  vprotect mnt -u 1ac068d3-4848-4c98-b30b-54ce050b6a95
  ```

## Tasks

Task management module is used to show and cancel tasks being executed. You can also monitor progress of the tasks.

**Note**:

* tasks in the queue are run in the order from the highest to the lowest priority.
* queue is being periodically cleaned, so only last tasks are being shown

To manage tasks use `vprotect task` sub-command.

```text
[root@vprotect ~]# vprotect task
Incorrect syntax: Missing required option: [-Q List queued tasks, -R List running tasks, -d Delete/cancel task, -F List finished (including failed) tasks, -g Show task details, -l List tasks (this node only), -L List tasks (all nodes and not assigned to any node)]

usage: task -d <GUID> | -F | -g <GUID> | -l | -L | -Q | -R
Task management
 -d,--delete <GUID>    Delete/cancel task
 -F,--list-finished    List finished (including failed) tasks
 -g,--details <GUID>   Show task details
 -l,--list             List tasks (this node only)
 -L,--list-all         List tasks (all nodes and not assigned to any node)
 -Q,--list-queued      List queued tasks
 -R,--list-running     List running tasks
```

### Examples

* To list tasks assigned to the current node:

  ```text
  vprotect task -l
  ```

* To list all tasks in the system:

  ```text
  vprotect task -L
  ```

* To show task details:

  ```text
  vprotect task -g 091d623d-edea-490f-8b3b-da30c09a3f93
  ```

* To cancel the task with ID `091d623d-edea-490f-8b3b-da30c09a3f93`:

  ```text
  vprotect task -d 091d623d-edea-490f-8b3b-da30c09a3f93
  ```

**Note:** some tasks may require to be finished before they are cancelled, i.e. export VM from the hypervisor – after cancellation it may take some time for task to process cancel request and clean up snapshots etc. if necessary - backup will be marked as failed.

## Application backup/restore

This module is used to manage backup and restore process. It is also used to list backups of a particular application.

To invoke backup and restore tasks use `vprotect brapp` sub-command.

```text
[root@vprotect ~]# vprotect brapp
Required option: [-b Backup (full), -B Backup (full) with task priority (0-100, 50 = default), -r Restore the backup, -T List tasks related to the backup, -gL Show file details, -I Restore and import backup. Enter D as path for DEFAULT source path from application, -gb Show backup details, -l List backups, -L List backup files]

usage: brapp -b <GUID> <BP_GUID | BP_NAME> | -B <GUID> <BP_GUID | BP_NAME> <PRIORITY> | -gb <BACKUP_GUID> | -gL <BACKUP_FILE_GUID> | -I <BACKUP_GUID>
       <DST_APP_GUID> <PATH> | -l | -L <GUID> | -r <GUID> <NODE_GUID | NODE_NAME> <DIRECTORY> | -T <GUID>
Application backup & restore
 -b,--backup <GUID> <BP_GUID | BP_NAME>                            Backup (full)
 -B,--backup-with-priority <GUID> <BP_GUID | BP_NAME> <PRIORITY>   Backup (full) with task priority (0-100, 50 = default)
 -gb,--show-backup-details <BACKUP_GUID>                           Show backup details
 -gL,--show-files-details <BACKUP_FILE_GUID>                       Show file details
 -I,--restore-and-import <BACKUP_GUID> <DST_APP_GUID> <PATH>       Restore and import backup. Enter D as path for DEFAULT source path from application
 -l,--list                                                         List backups
 -L,--list-files <GUID>                                            List backup files
 -r,--restore <GUID> <NODE_GUID | NODE_NAME> <DIRECTORY>           Restore the backup
 -T,--list-tasks <GUID>                                            List tasks related to the backup
```

### Examples

* To list all backups and their status:

  ```text
  vprotect brapp -l
  ```

* To show backups of a particular VM:

  ```text
  vprotect app -L 0f36f40c-6427-4035-9f2b-1ead6aca3597
  ```

* To show files that a specific backup consists of:

  ```text
  vprotect brapp -L 4f1c7907-72e9-4797-8470-3f1fbb081751
  ```

* To create a full backup of a app and store it in backup destination called `MyTSM`

  ```text
  vprotect brapp -b 0f36f40c-6427-4035-9f2b-1ead6aca3597 MyTSM
  ```

* To restore a full backup with given GUID on the current node \(`this`\) to the `/vprotect_data`

  ```text
  vprotect brapp -r 2132182d-e9ab-4478-a1db-48222b0e515b this /vprotect_data
  ```

* To restore application backup \(first GUID\) to the other application \(assuming it has REMOTE\_SSH in Command Execution Configuration - second GUID\)
* ```text
  vprotect brapp -H 2132182d-e9ab-4478-a1db-48222b0e515b c93140b8-a898-4aff-8eef-645587ca8289 "Local storage"
  ```

## Applications

Application management module is used to provide information about apps that has been defined report status of last backup of your apps \(and all backups for a particular app\).

To manage Applications in the system use `vprotect app` sub-command.

```text
[root@vprotect ~]# vprotect app
Required option: [-A Assign application backup policy, -dE Delete environment variable, -c Create a new application, -d Delete an application, -aE Add environment variable to application, -g Show application details, -xC Set application CMD exec configuration, -l List applications, -L List backups of the application, -m Modify application, -sC Set SSH access credentials, -sH Set SSH access host/port, -sK Set SSH key path, -mE Modify environment variable. Set variable hidden in UI(1) or visible(0), -lE List environment variables for application, -sN Set application node]

usage: app -A <GUID> <APP_POLICY_GUID> | -aE <APP_GUID> <NAME> <VALUE> | -c <NAME> <CONFIG_GUID> | -d <GUID> | -dE <GUID> | -g <GUID> | -l | -L <GUID> | -lE
       <APP_GUID> | -m <GUID> <NAME> | -mE <GUID> <APP_GUID> <NAME> <VALUE> <0|1> | -sC <GUID> <SSH_USER> <SSH_PASS> | -sH <GUID> <SSH_HOST> <SSH_PORT> | -sK
       <GUID> <SSH_KEYPATH> | -sN <GUID> <NODE_GUID> | -xC <GUID> <APP_CMD_EXEC_CONFIG_GUID>
Application backup management
 -A,--assign-app-policy <GUID> <APP_POLICY_GUID>                    Assign application backup policy
 -aE,--add-env-variable <APP_GUID> <NAME> <VALUE>                   Add environment variable to application
 -c,--create <NAME> <CONFIG_GUID>                                   Create a new application
 -d,--delete <GUID>                                                 Delete an application
 -dE,--delete-env-variable <GUID>                                   Delete environment variable
 -g,--details <GUID>                                                Show application details
 -l,--list                                                          List applications
 -L,--list-backups <GUID>                                           List backups of the application
 -lE,--list-env-variables <APP_GUID>                                List environment variables for application
 -m,--modify <GUID> <NAME>                                          Modify application
 -mE,--modify-env-variable <GUID> <APP_GUID> <NAME> <VALUE> <0|1>   Modify environment variable. Set variable hidden in UI(1) or visible(0)
 -sC,--set-ssh-credentials <GUID> <SSH_USER> <SSH_PASS>             Set SSH access credentials
 -sH,--set-ssh-host <GUID> <SSH_HOST> <SSH_PORT>                    Set SSH access host/port
 -sK,--set-ssh-key-path <GUID> <SSH_KEYPATH>                        Set SSH key path
 -sN,--set-node <GUID> <NODE_GUID>                                  Set application node
 -xC,--set-app-cmd-exec-config <GUID> <APP_CMD_EXEC_CONFIG_GUID>    Set application CMD exec configuration
```

### Examples

* To list all Applications

  ```text
  vprotect app -l
  ```

* To show details of the given Application \(by GUID\)

  ```text
  vprotect app -g 0f36f40c-6427-4035-9f2b-1ead6aca3597
  ```

* To add Application \(first GUID\) to the given policy \(second GUID\):

  ```text
  vprotect app -A 0f36f40c-6427-4035-9f2b-1ead6aca3597 3afcd507-a4f5-484d-8d34-53c73d7a5809
  ```

* To show backup history of a VM with given GUID:

  ```text
  vprotect app -L 0f36f40c-6427-4035-9f2b-1ead6aca3597
  ```

## Application backup policies

Application backup policies management module is used to define backup policies for Applications. You can assign different backup priority for a policy when the scheduler invokes backup task. You need first to define application backup policy and then add Applications to it. Application can belong only to a single backup policy.

To manage Application policies in the system use `vprotect apppolicy` sub-command.

**Note**: it is important to assign backup destination for a policy \(required for node to know where to store backups\)

Application backup policies are supposed to have multiple rules \(but currently vProtect supports exactly 1 rule\). Rule specifies schedules and backup destination for the policies.

```text
[root@vprotect ~]# vprotect apppolicy
Required option: [-p Set policy's backup task priority (0-100, 50 = default), -rR Remove rules from policy, -A Assign applications to policy, -aR Add new rule to selected policy, -r List rules for application backup policy, -c Create application backup policy, -d Delete a policy, -U Unassign applications from policy, -g Show details, -l List policies, -m Modify application backup policy]

usage: apppolicy -A <GUID> <APP_GUID,...,APP_GUID> | -aR <NAME> <POLICY_GUID> <SCHEDULE_GUID> <BACKUP_DESTINATION_GUID> | -c <NAME> <PRIORITY> | -d <GUID> |
       -g <GUID> | -l | -m <GUID> <NAME> <PRIORITY> | -p <GUID> <PRIORITY> | -r <GUID> | -rR <POLICY_GUID> <RULE_GUID,...,RULE_GUID> | -U <GUID>
       <APP_GUID,...,APP_GUID>
Application backup policy management
 -A,--assign applications <GUID> <APP_GUID,...,APP_GUID>                         Assign applications to policy
 -aR,--add-rule <NAME> <POLICY_GUID> <SCHEDULE_GUID> <BACKUP_DESTINATION_GUID>   Add new rule to selected policy
 -c,--create <NAME> <PRIORITY>                                                   Create application backup policy
 -d,--delete <GUID>                                                              Delete a policy
 -g,--details <GUID>                                                             Show details
 -l,--list                                                                       List policies
 -m,--modify <GUID> <NAME> <PRIORITY>                                            Modify application backup policy
 -p,--set-priority <GUID> <PRIORITY>                                             Set policy's backup task priority (0-100, 50 = default)
 -r,--list-rules <GUID>                                                          List rules for application backup policy
 -rR,--remove-rules <POLICY_GUID> <RULE_GUID,...,RULE_GUID>                      Remove rules from policy
 -U,--unassign applications <GUID> <APP_GUID,...,APP_GUID>                       Unassign applications from policy
```

### Examples

* To list all application backup policies

  ```text
  vprotect apppolicy -l
  ```

* To show details of the given application \(by GUID\)

  ```text
  vprotect apppolicy -g 3afcd507-a4f5-484d-8d34-53c73d7a5809
  ```

* To create a new policy

  ```text
  vprotect apppolicy -c "My Policy" 50
  ```

* To set backup destination for a policy:

  ```text
  Syntax: -aR <NAME> <POLICY_GUID> <SCHEDULE_GUID> <BACKUP_DESTINATION_GUID>
  vprotect apppolicy -aR MyPolicyRule d804439e-aeda-4fea-8f79-710dc355d2d1 1076134b-4e29-45ea-8c79-1e8ca216a3b8  9038d316-5d02-4972-913b-4201f4947178
  ```

## Application command execution management

Application command execution management module is used to define command execution for application. You can assign application to command definition or show details of command config. As you can see in the module syntax, you can do much more and we will show such operations in examples.

```text
[root@vprotect ~]# vprotect appconf
Required option: [-sS Set source path. Please remember that wildcard paths should be specified inside double quotes., -A Assign applications to config, -c Create application command execution config, timeout [min.], -d Delete application command execution config, -e Set export data to false(0) or true(1), -g Show details, -l List application command execution configs, -m Modify application command execution config", -sC Set CMD as semi-colon-separated string, i.e. "cmd;-a;-b", -r Remove files after export to false(0) or true(1), -sE Set handling for standard error. Values: DONT_IGNORE, IGNORE_WITH_WARNING, IGNORE_WITHOUT_WARNING, -t Set timeout in minutes, -U Unassign applications from config, -sI Set ignored command exit codes e.g. '15, 101-150' or '*']

usage: appconf -A <GUID> <APP_GUID,...,APP_GUID> | -c <NAME> <NODE | REMOTE_SSH> <FILE | STREAM> <TIMEOUT> | -d <GUID> | -e <GUID> <0|1>
       | -g <GUID> | -l | -m <GUID> <NAME> <NODE | REMOTE_SSH> <FILE | STREAM> | -r <GUID> <0|1> | -sC <GUID> <CMD_STRING> | -sE <GUID>
       <HANDLING> | -sI <GUID> <IGNORED_EXIT_CODES> | -sS <GUID> <SOURCE_PATH> | -t <GUID> <TIMEOUT> | -U <GUID> <APP_GUID,...,APP_GUID>
App command execution management
 -A,--assign-applications <GUID> <APP_GUID,...,APP_GUID>            Assign applications to config
 -c,--create <NAME> <NODE | REMOTE_SSH> <FILE | STREAM> <TIMEOUT>   Create application command execution config, timeout [min.]
 -d,--delete <GUID>                                                 Delete application command execution config
 -e,--set-export-data <GUID> <0|1>                                  Set export data to false(0) or true(1)
 -g,--details <GUID>                                                Show details
 -l,--list                                                          List application command execution configs
 -m,--modify <GUID> <NAME> <NODE | REMOTE_SSH> <FILE | STREAM>      Modify application command execution config"
 -r,--remove-after-export <GUID> <0|1>                              Remove files after export to false(0) or true(1)
 -sC,--set-cmd-arg <GUID> <CMD_STRING>                              Set CMD as semi-colon-separated string, i.e. "cmd;-a;-b"
 -sE,--set-std-error <GUID> <HANDLING>                              Set handling for standard error. Values: DONT_IGNORE,
                                                                    IGNORE_WITH_WARNING, IGNORE_WITHOUT_WARNING
 -sI,--set-ignored-codes <GUID> <IGNORED_EXIT_CODES>                Set ignored command exit codes e.g. '15, 101-150' or '*'
 -sS,--set-source-path <GUID> <SOURCE_PATH>                         Set source path. Please remember that wildcard paths should be
                                                                    specified inside double quotes.
 -t,--timeout <GUID> <TIMEOUT>                                      Set timeout in minutes
 -U,--unassign-applications <GUID> <APP_GUID,...,APP_GUID>          Unassign applications from config
```

### Examples

* To list all command configs:

```text
[root@vprotect ~]# vprotect appconf -l       

                GUID                             Name             CmdExecMethod  Applications  Export data  Remove files after export  Source type  
------------------------------------  --------------------------  -------------  ------------  -----------  -------------------------  -----------  
758112e8-2db6-46b1-8a65-ee7064f4933f  DB2                         Remote SSH     -             true         true                       File         
f807b3b1-7909-471f-8224-b8948dfca91f  file-backup                 Remote SSH     -             true         false                      File         
8b5e2b3f-3837-4f9d-a52a-fa7e9d0378fe  Kubernetes/OpenShift etcd   Remote SSH     -             true         true                       File         
9a6b1277-4388-42c3-8f81-ea4af6b0b27c  mysql                       Remote SSH     -             true         true                       File         
785ac097-cd1d-43e3-92c3-ac42d710adb3  MySQL/MariaDB               Remote SSH     -             true         true                       File         
49fa4b52-0fda-44db-a692-7ee8ed071b86  MySQL/MariaDB_2             Remote SSH     -             true         true                       File
```

* To see command config details:

```text
[root@vprotect ~]# vprotect appconf -g 9a6b1277-4388-42c3-8f81-ea4af6b0b27c
Property                   Value                                                             
-------------------------  ------------------------------------------------------------  

GUID                       9a6b1277-4388-42c3-8f81-ea4af6b0b27c  
Name                       mysql  
CmdArg                     [$VP_MYSQL_SCRIPTPATH/vp_backup_mysql.sh, >>, $VP_MYSQL_LOGFILE]  
Applications               []  
Cmd execution method       REMOTE_SSH  
Export data                true  
Remove files after export  true  
Source type                FILE  
Source path                $VP_MYSQL_STOREPATH/*  
Timeout in minutes         60  
Ignored exit codes         -  
Standard error handling    DONT_IGNORE
```

## Restore Jobs

You can use this small module to list restore job operations and see details of every task.

```text
[root@vprotect ~]# vprotect restorejob
Incorrect syntax: Missing required option: [-g Show restore job details, -l List restore jobs]

usage: restorejob -g <RESTOREJOB_GUID> | -l
Restore jobs
 -g,--show-restore-job-details <RESTOREJOB_GUID>   Show restore job details
 -l,--list                                         List restore jobs
```

### Examples

* To list restore jobs:

```text
 [root@vprotect ~]# vprotect restorejob -l    

                GUID                     Restore Type     Status                                                                                           Status info                                                                                           VM/APP  
------------------------------------  ------------------  -------  --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------  ------  
e9aff98f-e431-467f-b9f9-46b2f86068eb  Restore and mount   Success  Backups of VM: [PM]Windows_2016 (size: 50 GiB) mounted in: 1m 14s.                                                                                                                            -       
0dcdfed3-f793-4b9f-8faf-a608396de8e8  Restore             Failed   ExternalAPIException: Failed to create directory: /vprotect_data/import/a2778f74-dc6c-4a1d-88ea-1f85361f4fbd/VM_01_Apine/2020-06-08__11.50.                                                   -       
fd99c9ec-ac25-448c-a973-d654936076b2  Restore             Failed   Backup destination is not assigned to the node                                                                                                                                                -       
f23c2c8c-2bec-4e8e-9ba9-2c6251434907  Restore and mount   Success  Backups of VM: alpine (size: 2.3 GiB) mounted in: 1m 31s
a24f15c0-617f-4808-869a-ed346e30a713  Restore and import  Success  Backups of VM: Alpine-Linux (size: 2 GiB) imported in: 3m 5s.
```

* To see restore job details:

```text
[root@vprotect ~]# vprotect restorejob -g e9aff98f-e431-467f-b9f9-46b2f86068eb
Property                         Value                                                               
-------------------------------  ------------------------------------------------------------  

GUID                             e9aff98f-e431-467f-b9f9-46b2f86068eb  
Restore Type                     RESTORE_AND_MOUNT  
Status                           SUCCESS  
Status info                      Backups of VM: [PM]Windows_2016 (size: 50 GiB) mounted in: 1m 14s.  
VM/APP                           -  
Tasks                            0  
Backup                           1906e226-2ba6-439a-b723-31efb6e5196a  
Destination VM/APP               [PM]Windows_2016 (f2354325-562e-465b-abc4-c4db51036b80)  
Restore Storage ID               -  
Restore Cluster ID               -  
Restored VM/APP Name             -  
Restore Project Name             -  
Restore Path                     -  
Tenant ID                        -  
Base Image                       -  
Data Center Name                 -  
Restored Disk Allocation Format  -  
Mounted Backup Mode              AUTO  
Backup Type                      FULL  
Node                             vPro-Local (b46043f2-df06-482c-b4ba-37fc8093022c)
```

## Recovery plans policies

Recovery plans policies module is used to define recovery plans policies and rules for VMs. You can assign different restore priority for a policy when the scheduler invokes restore task. You need first to create a new policy and then add new rules to it.

```text
[root@vprotect ~]# vprotect recplan
Required option: [-sRrci Set restore cluster id in rule, -sRa Set rule status, -sRhvmt Set hypervisor manager type in rule, -sRuag Set rule use auto generated name, -sRrp Set restore path in rule, -sRdc Set data center in rule, -sRn Set node in rule, -sRrn Set target VM name in rule, -sRtp Set target project in rule, -aR Add new rule to selected policy, -sRdaf Set disk allocation format in rule, -sRrsi Set restore storage id in rule, -sRv Add virtual machines to rule, -rR Remove rules from policy, -c Create a new policy, -d Delete a policy, -sRbs Set backup selection in rule, -g Show details, -sRovw Set overwrite status in rule, -l List policies, -m Modify policy, -p Set policy's backup task priority (0-100, 50 = default), -r List rules for policy, -srBi Set base image in rule, -sRhv Set hypervisor in rule, -sRhvm Set hypervisor manager in rule, -sRhvt Set hypervisor type in rule]

usage: recplan -aR <NAME> <POLICY_GUID>> | -c <NAME> | -d <GUID> | -g <GUID> | -l | -m <GUID> <NAME> | -p <GUID> <PRIORITY> | -r <GUID> |
       -rR <POLICY_GUID> <RULE_GUID,...,RULE_GUID> | -sRa <RULE_GUID> <TRUE|FALSE>> | -srBi <RULE_GUID> <BASE_IMAGE_UUID>> | -sRbs
       <RULE_GUID> <LAST_SUCCESSFUL | ANY>> | -sRdaf <RULE_GUID> <PREALLOCATED | SPARSE>> | -sRdc <RULE_GUID> <DATA_CENTER_NAME>> | -sRhv
       <RULE_GUID> <HYPERVISOR_GUID>> | -sRhvm <RULE_GUID> <HYPERVISORMANAGER_GUID>> | -sRhvmt <RULE_GUID> <HYPERVISORMANAGERTYPE>> |
       -sRhvt <RULE_GUID> <HYPERVISORTYPE>> | -sRn <RULE_GUID> <NODE_GUID>> | -sRovw <RULE_GUID> <TRUE|FALSE>> | -sRrci <RULE_GUID>
       <RESTORE_CLUSTER_ID>> | -sRrn <RULE_GUID> <VM_NAME>> | -sRrp <RULE_GUID> <PATH>> | -sRrsi <RULE_GUID> <RESTORE_STORAGE_ID>> |
       -sRtp <RULE_GUID> <PROJECT_NAME>> | -sRuag <RULE_GUID> <TRUE|FALSE>> | -sRv <RULE_GUID> <VM_GUIDS>>
Recovery plan policies
 -aR,--add-rule <NAME> <POLICY_GUID>>                                            Add new rule to selected policy
 -c,--create <NAME>                                                              Create a new policy
 -d,--delete <GUID>                                                              Delete a policy
 -g,--details <GUID>                                                             Show details
 -l,--list                                                                       List policies
 -m,--modify <GUID> <NAME>                                                       Modify policy
 -p,--set-priority <GUID> <PRIORITY>                                             Set policy's backup task priority (0-100, 50 = default)
 -r,--list-rules <GUID>                                                          List rules for policy
 -rR,--remove-rules <POLICY_GUID> <RULE_GUID,...,RULE_GUID>                      Remove rules from policy
 -sRa,--set-rule-active <RULE_GUID> <TRUE|FALSE>>                                Set rule status
 -srBi,--set-rule-base-image <RULE_GUID> <BASE_IMAGE_UUID>>                      Set base image in rule
 -sRbs,--set-rule-backup-selection <RULE_GUID> <LAST_SUCCESSFUL | ANY>>          Set backup selection in rule
 -sRdaf,--set-rule-disk-allocation-format <RULE_GUID> <PREALLOCATED | SPARSE>>   Set disk allocation format in rule
 -sRdc,--set-rule-data-center <RULE_GUID> <DATA_CENTER_NAME>>                    Set data center in rule
 -sRhv,--set-rule-hv <RULE_GUID> <HYPERVISOR_GUID>>                              Set hypervisor in rule
 -sRhvm,--set-rule-hvm <RULE_GUID> <HYPERVISORMANAGER_GUID>>                     Set hypervisor manager in rule
 -sRhvmt,--set-rule-hvm-type <RULE_GUID> <HYPERVISORMANAGERTYPE>>                Set hypervisor manager type in rule
 -sRhvt,--set-rule-hv-type <RULE_GUID> <HYPERVISORTYPE>>                         Set hypervisor type in rule
 -sRn,--set-rule-node <RULE_GUID> <NODE_GUID>>                                   Set node in rule
 -sRovw,--set-rule-overwrite <RULE_GUID> <TRUE|FALSE>>                           Set overwrite status in rule
 -sRrci,--set-rule-restore-cluster-id <RULE_GUID> <RESTORE_CLUSTER_ID>>          Set restore cluster id in rule
 -sRrn,--set-rule-restored-name <RULE_GUID> <VM_NAME>>                           Set target VM name in rule
 -sRrp,--set-rule-restore-path <RULE_GUID> <PATH>>                               Set restore path in rule
 -sRrsi,--set-rule-restore-storage-id <RULE_GUID> <RESTORE_STORAGE_ID>>          Set restore storage id in rule
 -sRtp,--set-rule-target-project <RULE_GUID> <PROJECT_NAME>>                     Set target project in rule
 -sRuag,--set-rule-use-auto-generated-name <RULE_GUID> <TRUE|FALSE>>             Set rule use auto generated name
 -sRv,--set-rule-vms <RULE_GUID> <VM_GUIDS>>                                     Add virtual machines to rule
```

### Examples

* To create new policy:

```text
[root@vprotect ~]# vprotect recplan -c "New Recovery Policy"

                GUID                         Name          Priority  Rules  
------------------------------------  -------------------  --------  -----  
d76bb990-817d-47ca-ba04-1dbdeffacf2a  New Recovery Policy  50        0
```

* To add new rule to selected policy:

```text
[root@vprotect ~]# vprotect recplan -aR Rule1 d76bb990-817d-47ca-ba04-1dbdeffacf2a
Property      Value                                 
------------  ------------------------------------  

GUID          d76bb990-817d-47ca-ba04-1dbdeffacf2a  
Name          New Recovery Policy  
Priority      50  
No. of rules  1
```

* To list policies:

```text
[root@vprotect ~]# vprotect recplan -l

                GUID                         Name          Priority  Rules  
------------------------------------  -------------------  --------  -----  
d76bb990-817d-47ca-ba04-1dbdeffacf2a  New Recovery Policy  50        1
```

* To list rules assigned to policy:

```text
[root@vprotect ~]# vprotect recplan -r d76bb990-817d-47ca-ba04-1dbdeffacf2a

                GUID                  Name   VMs  Hypervisor Type  Hypervisor Manager Type  Hypervisor  Hypervisor Manager  Node  Backup Selection  Active  Restore Storage Id  Restore Cluster Id  Target VM Name  Target Project  Restore Path  Overwrite  Base Image  Data Center  Disk Allocation Format  Use auto generated name  
------------------------------------  -----  ---  ---------------  -----------------------  ----------  ------------------  ----  ----------------  ------  ------------------  ------------------  --------------  --------------  ------------  ---------  ----------  -----------  ----------------------  -----------------------  
6f0cc9ae-5842-4339-aee1-c8d21904317b  Rule1  []   -                -                        -           -                   -     Last successful   true    null  null  null  null  null  true       -           -            -                       false
```

* To select the virtual machine you want to restore using a rule :

```text
[root@vprotect ~]# vprotect vm -l      # To list virtual machines and their GUID's

[root@vprotect ~]# vprotect recplan -sRv 6f0cc9ae-5842-4339-aee1-c8d21904317b f2354325-562e-465b-abc4-c4db51036b80

                GUID                         Name          Priority  Rules  
------------------------------------  -------------------  --------  -----  
d76bb990-817d-47ca-ba04-1dbdeffacf2a  New Recovery Policy  50        1
```

