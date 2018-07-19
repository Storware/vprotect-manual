# Virtual machines

Virtual machine management module is used to provide information about VMs that has been detected on hypervisors, report status of last backup of your VMs \(and all backups for a particular VM\) and set priority for operations invoked on VM.

To manage VMs in the system use `vprotect vm` sub-command.

VMs are detected automatically during `Index` task executed on the HV or HV manager.

```text
[root@localhost ~]# vprotect vm 
Incorrect syntax: Missing required option: [-A Assign VM to the group, -d Delete VM, -D List detected VM disks, -g Show virtual machine details, -xC Set pre-snapshot CMD as semi-colon-separated string, i.e. "cmd;-a;-b", -XC Set post-snapshot CMD as semi-colon-separated string, i.e. "cmd;-a;-b", -wb Acknowledge warnings related to the backup, -l List VMs, -L List backups of the VM, -xE Set pre-snapshot CMD exec enabled (1) / disabled (0), -XE Set post-snapshot CMD exec enabled (1) / disabled (0), -sC Set SSH access credentials, -S List managed VM snapshots, -T List tasks related to the VM, -sH Set SSH access host/port, -w Acknowledge all backup warnings related to the VM, -gb Show backup details]

usage: vm -A <GUID> <VM_GROUP_GUID> | -d <GUID> | -D <GUID> | -g <<VM_GUID>> | -gb <<BACKUP_GUID>> | -l | -L <GUID> | -S <GUID> | -sC <GUID> <SSH_USER> <SSH_PASS> | -sH <GUID> <SSH_HOST>
       <SSH_PORT> | -T <GUID> | -w <GUID> | -wb <BACKUP_GUID> | -xC <GUID> <CMD_STRING> | -XC <GUID> <CMD_STRING> | -xE <GUID> <0|1> | -XE <GUID> <0|1>
Virtual machine management
 -A,--assign-vmg <GUID> <VM_GROUP_GUID>                   Assign VM to the group
 -d,--delete <GUID>                                       Delete VM
 -D,--list-disks <GUID>                                   List detected VM disks
 -g,--details <<VM_GUID>>                                 Show virtual machine details
 -gb,--show-backup-details <<BACKUP_GUID>>                Show backup details
 -l,--list                                                List VMs
 -L,--list-backups <GUID>                                 List backups of the VM
 -S,--list-snapshots <GUID>                               List managed VM snapshots
 -sC,--set-ssh-credentials <GUID> <SSH_USER> <SSH_PASS>   Set SSH access credentials
 -sH,--set-ssh-host <GUID> <SSH_HOST> <SSH_PORT>          Set SSH access host/port
 -T,--list-tasks <GUID>                                   List tasks related to the VM
 -w,--ack-all-backup-warnings <GUID>                      Acknowledge all backup warnings related to the VM
 -wb,--ack-backup-warnings <BACKUP_GUID>                  Acknowledge warnings related to the backup
 -xC,--set-pre-snap-cmd <GUID> <CMD_STRING>               Set pre-snapshot CMD as semi-colon-separated string, i.e. "cmd;-a;-b"
 -XC,--set-post-snap-cmd <GUID> <CMD_STRING>              Set post-snapshot CMD as semi-colon-separated string, i.e. "cmd;-a;-b"
 -xE,--set-pre-snap-cmd-exec-enabled <GUID> <0|1>         Set pre-snapshot CMD exec enabled (1) / disabled (0)
 -XE,--set-post-snap-cmd-exec-enabled <GUID> <0|1>        Set post-snapshot CMD exec enabled (1) / disabled (0)
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

