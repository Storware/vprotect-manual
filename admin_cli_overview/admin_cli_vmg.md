# VM backup policies

Virtual machine backup policies management module is used to define backup policies of VMs. You can assign different backup priority for a policy when the scheduler invokes backup task. You need first to define a VM policy and then add VMs to it. VM can belong only to a single backup policy.

To manage VM policies in the system use `vprotect vmpolicy` sub-command.

VMs are assigned automatically to the policy only if VM has no policy assigned already. If automatic assignment has been turned on for a policy and either name of the VM matches regular expression, or tag detected \(Citrix/oVirt/RHV/Oracle VM\) matches tag defined for the policy, VM is assigned to the policy, and all schedules for a policy will also be automatically invoked for this VM.

**Note**: it is important to assign backup destination for a policy \(required for node to know where to store backups\)

```text
[root@localhost vprotect]# vprotect vmpolicy
Incorrect syntax: Missing required option: [-rR Remove auto-assignment RE, -b Set backup destination for the VM policy, -rT Remove auto-assignment tag, -aC Set auto-assignment HV clusters, -c Create a new policy, -d Delete a policy, -g Show hypervisor details, -l List policies, -L List VMs in the policy, -aM Set auto-assignment mode, -m Modify policy, -aN Set auto-removal of non-present VMs flag, -p Set policy's backup task priority (0-100, 50 = default), -aR Add auto-assignment RE, -S List schedules for the policy, -s Set schedules for the VM policy, -aT Add auto-assignment tag, -U Unassign VMs from the policy, -V Assign VMs to the policy]

usage: vmpolicy -aC <GUID > <[HV_CLUSTER_GUID,...,HV_CLUSTER_GUID]> | -aM <GUID> <DISABLED|ASSIGN_ONLY|ASSIGN_AND_REMOVE> | -aN <GUID> <0|1> | -aR <GUID> <inc|exc> <REG_EXP> | -aT <GUID> <inc|exc> <TAG> | -b
       <GUID> <BD_GUID | BD_NAME>> | -c <NAME> | -d <GUID> | -g <GUID> | -l | -L <GUID> | -m <GUID> <NAME> | -p <GUID> <PRIORITY> | -rR <GUID> <inc|exc> <REG_EXP> | -rT <GUID> <inc|exc> <TAG> | -S <GUID> | -s
       <GUID> <SCHED_GUID,...,SCHED_GUID> | -U <VM_GUID,...,VM_GUID> | -V <GUID> <VM_GUID,...,VM_GUID>
VM backup policy management
 -aC,--set-auto-assign-hv-clusters <GUID > <[HV_CLUSTER_GUID,...,HV_CLUSTER_GUID]>   Set auto-assignment HV clusters
 -aM,--set-auto-assign-mode <GUID> <DISABLED|ASSIGN_ONLY|ASSIGN_AND_REMOVE>          Set auto-assignment mode
 -aN,--set-auto-remove-non-present <GUID> <0|1>                                      Set auto-removal of non-present VMs flag
 -aR,--add-auto-assign-re <GUID> <inc|exc> <REG_EXP>                                 Add auto-assignment RE
 -aT,--add-auto-assign-tag <GUID> <inc|exc> <TAG>                                    Add auto-assignment tag
 -b,--set-backup-destination <GUID> <BD_GUID | BD_NAME>>                             Set backup destination for the VM policy
 -c,--create <NAME>                                                                  Create a new policy
 -d,--delete <GUID>                                                                  Delete a policy
 -g,--details <GUID>                                                                 Show hypervisor details
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

## Examples

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



