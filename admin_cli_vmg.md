# Virtual machine group management

Virtual machine group management module is used to define groups of VMs. You can assign different backup priority for a group when the scheduler invokes backup task. You need first to define a VM group and then add VMs to it. VM can belong only to a single group.

To manage VMs in the system use `vprotect vmg` sub-command. 

VMs are assigned automatically to the group only if VM has no group assigned already. If automatic assignment has been turned on for a group and either name of the VM matches regular expression, or tag detected (Citrix/oVirt/RHV) matches tag defined for the group, VM is assigned to the group, and all schedules for a group will also be automatically invoked for this VM.

**Note**: it is important to assigne backup destination for a group (required for node to know where to store backups)

```
[root@localhost vprotect]# vprotect vmg
Incorrect syntax: Missing required option: [-rR Remove auto-assignment RE, -b Set backup destination for the VM group, -rT Remove auto-assignment tag, -c Create a new group, -d Delete a group, -g Show hypervisor details, -l List groups, -L List VMs in the group, -aM Set auto-assignment mode, -m Modify group, -aN Set auto-removal of non-present VMs flag, -p Set group's backup task priority (0-100, 50 = default), -aR Add auto-assignment RE, -S List schedules for the group, -s Set schedules for the VM group, -aT Add auto-assignment tag, -U Unassign VMs from the group, -V Assign VMs to the group]

usage: vmg -aM <GUID> <DISABLED|ASSIGN_ONLY|ASSIGN_AND_REMOVE> | -aN <GUID> <0|1> | -aR <GUID> <inc|exc> <REG_EXP> | -aT <GUID> <inc|exc> <TAG> | -b <GUID> <BD_GUID | BD_NAME>> | -c <NAME> | -d <GUID> | -g
       <GUID> | -l | -L <GUID> | -m <GUID> <NAME> | -p <GUID> <PRIORITY> | -rR <GUID> <inc|exc> <REG_EXP> | -rT <GUID> <inc|exc> <TAG> | -S <GUID> | -s <GUID> <SCHED_GUID,...,SCHED_GUID> | -U <GUID>
       <VM_GUID,...,VM_GUID> | -V <GUID> <VM_GUID,...,VM_GUID>
VM group management
 -aM,--set-auto-assign-mode <GUID> <DISABLED|ASSIGN_ONLY|ASSIGN_AND_REMOVE>   Set auto-assignment mode
 -aN,--set-auto-remove-non-present <GUID> <0|1>                               Set auto-removal of non-present VMs flag
 -aR,--add-auto-assign-re <GUID> <inc|exc> <REG_EXP>                          Add auto-assignment RE
 -aT,--add-auto-assign-tag <GUID> <inc|exc> <TAG>                             Add auto-assignment tag
 -b,--set-backup-destination <GUID> <BD_GUID | BD_NAME>>                      Set backup destination for the VM group
 -c,--create <NAME>                                                           Create a new group
 -d,--delete <GUID>                                                           Delete a group
 -g,--details <GUID>                                                          Show hypervisor details
 -l,--list                                                                    List groups
 -L,--list-vms <GUID>                                                         List VMs in the group
 -m,--modify <GUID> <NAME>                                                    Modify group
 -p,--set-priority <GUID> <PRIORITY>                                          Set group's backup task priority (0-100, 50 = default)
 -rR,--remove-auto-assign-re <GUID> <inc|exc> <REG_EXP>                       Remove auto-assignment RE
 -rT,--remove-auto-assign-tag <GUID> <inc|exc> <TAG>                          Remove auto-assignment tag
 -S,--list-schedules <GUID>                                                   List schedules for the group
 -s,--set-schedules <GUID> <SCHED_GUID,...,SCHED_GUID>                        Set schedules for the VM group
 -U,--unassign-vms <GUID> <VM_GUID,...,VM_GUID>                               Unassign VMs from the group
 -V,--assign-vms <GUID> <VM_GUID,...,VM_GUID>                                 Assign VMs to the group
```	

## Examples
* To list all VM groups

  ```
vprotect vmg -l
  ```
	
* To show details of the given VM (by GUID)

  ```
vprotect vmg -g 3afcd507-a4f5-484d-8d34-53c73d7a5809
  ```
	
* To create a new group

  ```
vprotect vmg -c "My Group" 
  ```

* To add `group1` tag (include) for a group:
	
  ```
vprotect vmg -aT 3afcd507-a4f5-484d-8d34-53c73d7a5809 inc group1
  ```
  
* To remove `group1` tag (include) for a group:
	
  ```
vprotect vmg -rT 3afcd507-a4f5-484d-8d34-53c73d7a5809 inc group1
  ```
	
* To set 2 schedules (GUIDs are comma-separated) for a group (first GUID):
	
  ```
vprotect vmg -s 3afcd507-a4f5-484d-8d34-53c73d7a5809 391203e3-ad6c-4532-b69c-78b3a5cf4ef5,e4c1c61d-26db-4e41-87cf-2195d5498cde
  ```
  
* To set backup destination (you can use name or GUID) for a group (first GUID):
	
  ```
vprotect vmg -b 3afcd507-a4f5-484d-8d34-53c73d7a5809 ISP
  ```
