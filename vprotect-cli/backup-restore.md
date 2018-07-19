# Backup/restore

This module is used to manage backup and restore process. It is also used to list backups of a particular VM.

To invoke backup and restore tasks use `vprotect br` sub-command.

```text
[root@localhost vprotect]# vprotect br
Incorrect syntax: Missing required option: [-b Backup VM (full), -B Backup VM (full) with task priority (0-100, 50 = default), -gL Show file details, -F List file systems, -H Restore the backup to the hypervisor, -i Backup VM (incremental), -I Backup VM (incremental) with task priority (0-100, 50 = default), -l List backups, -L List backup files, -M Restore the backup to the hypervisor manager, -r Restore the backup, -T List tasks related to the backup, -gb Show backup details]

usage: br -b <VM_GUID> <BP_GUID | BP_NAME> | -B <VM_GUID> <BP_GUID | BP_NAME> <PRIORITY> | -F <GUID> | -gb <BACKUP_GUID> | -gL <BACKUP_FILE_GUID> | -H <GUID> <HV_GUID | HV_HOST> <STORAGE_ID> | -i <VM_GUID>
       <BP_GUID | BP_NAME> | -I <VM_GUID> <BP_GUID | BP_NAME> <PRIORITY> | -l | -L <GUID> | -M <GUID> <HVM_GUID | HVM_HOST> <STORAGE_ID> | -r <GUID> <NODE_GUID | NODE_NAME> <DIRECTORY> | -T <GUID>
Backup/restore
 -b,--backup <VM_GUID> <BP_GUID | BP_NAME>                                Backup VM (full)
 -B,--backup-with-priority <VM_GUID> <BP_GUID | BP_NAME> <PRIORITY>       Backup VM (full) with task priority (0-100, 50 = default)
 -F,--list-file-systems <GUID>                                            List file systems
 -gb,--show-backup-details <BACKUP_GUID>                                  Show backup details
 -gL,--show-files-details <BACKUP_FILE_GUID>                              Show file details
 -H,--restore-to-hv <GUID> <HV_GUID | HV_HOST> <STORAGE_ID>               Restore the backup to the hypervisor
 -i,--backup-inc <VM_GUID> <BP_GUID | BP_NAME>                            Backup VM (incremental)
 -I,--backup-inc-with-priority <VM_GUID> <BP_GUID | BP_NAME> <PRIORITY>   Backup VM (incremental) with task priority (0-100, 50 = default)
 -l,--list                                                                List backups
 -L,--list-files <GUID>                                                   List backup files
 -M,--restore-to-hvm <GUID> <HVM_GUID | HVM_HOST> <STORAGE_ID>            Restore the backup to the hypervisor manager
 -r,--restore <GUID> <NODE_GUID | NODE_NAME> <DIRECTORY>                  Restore the backup
 -T,--list-tasks <GUID>                                                   List tasks related to the backup
```

## Examples

* To list all backups and their status:

  ```text
  vprotect br -l
  ```

* To show backups of a particular VM:

  ```text
  vprotect vm -L 0f36f40c-6427-4035-9f2b-1ead6aca3597
  ```

* To show files that a specific backup consists of:

  ```text
  vprotect br -L 4f1c7907-72e9-4797-8470-3f1fbb081751
  ```

* To show detected file systems in specific backup:

  ```text
  vprotect br -F 4f1c7907-72e9-4797-8470-3f1fbb081751
  ```

* To create a full backup of a VM and store it in backup destination called `MyTSM`

  ```text
  vprotect br -b 0f36f40c-6427-4035-9f2b-1ead6aca3597 MyTSM
  ```

* To restore a full backup with given GUID on the current node \(`this`\) to the `/vprotect_data`

  ```text
  vprotect br -r 2132182d-e9ab-4478-a1db-48222b0e515b this /vprotect_data
  ```

* To create an incremental backup \(Citrix only\) of a VM and store it in backup destination called `MyTSM`

  ```text
  vprotect br -i 0f36f40c-6427-4035-9f2b-1ead6aca3597 MyTSM
  ```

* To restore VM backup \(first GUID\) to the hypervisor \(second GUID - Citrix XenServer only\) and specific storage

  ```text
  vprotect br -H 2132182d-e9ab-4478-a1db-48222b0e515b c93140b8-a898-4aff-8eef-645587ca8289 "Local storage"
  ```

