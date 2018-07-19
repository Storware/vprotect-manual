# Hypervisor cluster management

Hypervisor clusters management module enables you to view and remove clsters detected on RHV/oVirt/Nutanix/OVM/XenServer enviornments.

To view or delete them use `vprotect hc` sub-command. 

```
[root@localhost ~]# vprotect hc
Incorrect syntax: Missing required option: [-d Delete HV cluster, -l List HV clusters]

usage: hc -d <GUID> | -l
Hypervisor cluster management
 -d,--delete <GUID>   Delete HV cluster
 -l,--list            List HV clusters
```	

## Examples
* To list all detected clusters:

  ```
vprotect hc -l
  ```
	* To delete a cluster with GUID `107bc87a-9adf-4d6c-b732-345dd06c59e9`: 
  ```
vprotect hc -d 107bc87a-9adf-4d6c-b732-345dd06c59e9
  ```