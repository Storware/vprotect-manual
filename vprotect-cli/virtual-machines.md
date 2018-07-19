# Virtual machines

Virtual machine management module is used to provide information about VMs that has been detected on hypervisors, report status of last backup of your VMs \(and all backups for a particular VM\) and set priority for operations invoked on VM.

To manage VMs in the system use `vprotect vm` sub-command.

VMs are detected automatically during `Index` task executed on the HV or HV manager.

```text
[root@localhost vprotect]# vprotect vm
Incorrect syntax: Missing required option: [-A Assign VM to the group, -S List managed VM snapshots, -d Delete VM, -D List detected VM disks, -T List tasks related to the VM, -g Show virtual machine details, -gb Show backup details, -l List VMs, -L List backups of the VM]

usage: vm -A <GUID> <VM_GROUP_GUID> | -d <GUID> | -D <GUID> | -g <<VM_GUID>> | -gb <<BACKUP_GUID>> | -l | -L <GUID> | -S <GUID> | -T <GUID>
Virtual machine management
 -A,--assign-vmg <GUID> <VM_GROUP_GUID>      Assign VM to the group
 -d,--delete <GUID>                          Delete VM
 -D,--list-disks <GUID>                      List detected VM disks
 -g,--details <<VM_GUID>>                    Show virtual machine details
 -gb,--show-backup-details <<BACKUP_GUID>>   Show backup details
 -l,--list                                   List VMs
 -L,--list-backups <GUID>                    List backups of the VM
 -S,--list-snapshots <GUID>                  List managed VM snapshots
 -T,--list-tasks <GUID>                      List tasks related to the VM
```

## Examples

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

