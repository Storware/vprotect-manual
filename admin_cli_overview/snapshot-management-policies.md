# Snapshot management policies

Snapshot management policies module enables CDM for your VMs and manages their retention. Currently it is supported for KVM, Citrix, RHV/oVirt platforms.

To manage snapshot policies in the system use `vprotect snappolicy` sub-command.

VMs are assigned automatically to the policy only if VM has no policy assigned already. If automatic assignment has been turned on for a policy and either name of the VM matches regular expression, or tag detected \(Citrix/oVirt/RHV\) matches tag defined for the policy, VM is assigned to the policy, and all schedules for a policy will also be automatically invoked for this VM.

Notice: only VMs with assigned snapshot management policy can be snapshoted from the CLI or UI

```text
[root@vprotect ~]# vprotect snappolicy
Incorrect syntax: Missing required option: [-rR Remove auto-assignment RE, -rT Remove auto-assignment tag, -aC Set auto-assignment HV clusters, -c Create a new policy, -d Delete a policy, -g Show policy details, -l List policies, -L List VMs in the policy, -aM Set auto-assignment mode, -m Modify policy, -aN Set auto-removal of non-present VMs flag, -p Set policy's backup task priority (0-100, 50 = default), -aR Add auto-assignment RE, -r List rules for policy, -aT Add auto-assignment tag, -U Unassign VMs from the policy, -V Assign VMs to the policy]

usage: snappolicy -aC <GUID > <[HV_CLUSTER_GUID,...,HV_CLUSTER_GUID]> | -aM <GUID> <DISABLED|ASSIGN_ONLY|ASSIGN_AND_REMOVE> | -aN <GUID> <0|1> | -aR <GUID> <inc|exc> <REG_EXP> | -aT <GUID> <inc|exc> <TAG> | -c
       <NAME> | -d <GUID> | -g <GUID> | -l | -L <GUID> | -m <GUID> <NAME> | -p <GUID> <PRIORITY> | -r <GUID> | -rR <GUID> <inc|exc> <REG_EXP> | -rT <GUID> <inc|exc> <TAG> | -U <VM_GUID,...,VM_GUID> | -V <GUID>
       <VM_GUID,...,VM_GUID>
Snapshot policy management
 -aC,--set-auto-assign-hv-clusters <GUID > <[HV_CLUSTER_GUID,...,HV_CLUSTER_GUID]>   Set auto-assignment HV clusters
 -aM,--set-auto-assign-mode <GUID> <DISABLED|ASSIGN_ONLY|ASSIGN_AND_REMOVE>          Set auto-assignment mode
 -aN,--set-auto-remove-non-present <GUID> <0|1>                                      Set auto-removal of non-present VMs flag
 -aR,--add-auto-assign-re <GUID> <inc|exc> <REG_EXP>                                 Add auto-assignment RE
 -aT,--add-auto-assign-tag <GUID> <inc|exc> <TAG>                                    Add auto-assignment tag
 -c,--create <NAME>                                                                  Create a new policy
 -d,--delete <GUID>                                                                  Delete a policy
 -g,--details <GUID>                                                                 Show policy details
 -l,--list                                                                           List policies
 -L,--list-vms <GUID>                                                                List VMs in the policy
 -m,--modify <GUID> <NAME>                                                           Modify policy
 -p,--set-priority <GUID> <PRIORITY>                                                 Set policy's backup task priority (0-100, 50 = default)
 -r,--list-rules <GUID>                                                              List rules for policy
 -rR,--remove-auto-assign-re <GUID> <inc|exc> <REG_EXP>                              Remove auto-assignment RE
 -rT,--remove-auto-assign-tag <GUID> <inc|exc> <TAG>                                 Remove auto-assignment tag
 -U,--unassign-vms <VM_GUID,...,VM_GUID>                                             Unassign VMs from the policy
 -V,--assign-vms <GUID> <VM_GUID,...,VM_GUID>                                        Assign VMs to the policy
```



## Examples

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

