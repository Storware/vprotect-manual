# Application backup/restore

This module is used to manage backup and restore process. It is also used to list backups of a particular application.

To invoke backup and restore tasks use `vprotect brapp` sub-command.

```text
[root@vprotect ~]# vprotect brapp
Incorrect syntax: Missing required option: [-b Backup Application, -B Backup Application with task priority (0-100, 50 = default), -r Restore the backup, -T List tasks related to the backup, -gL Show file details, -I Restore and import backup. Enter D as path for DEFAULT source path from application, -gb Show backup details, -l List backups, -L List backup files]

usage: brapp -b <APP_GUID> <BP_GUID | BP_NAME> | -B <APP_GUID> <BP_GUID | BP_NAME> <PRIORITY> | -gb <BACKUP_GUID> | -gL <BACKUP_FILE_GUID> | -I <BACKUP_GUID> <DST_APP_GUID> <PATH> | -l | -L <GUID> | -r <GUID>
       <NODE_GUID | NODE_NAME> <DIRECTORY> | -T <GUID>
Application backup & restore
 -b,--backup <APP_GUID> <BP_GUID | BP_NAME>                            Backup Application
 -B,--backup-with-priority <APP_GUID> <BP_GUID | BP_NAME> <PRIORITY>   Backup Application with task priority (0-100, 50 = default)
 -gb,--show-backup-details <BACKUP_GUID>                               Show backup details
 -gL,--show-files-details <BACKUP_FILE_GUID>                           Show file details
 -I,--restore-and-import <BACKUP_GUID> <DST_APP_GUID> <PATH>           Restore and import backup. Enter D as path for DEFAULT source path from application
 -l,--list                                                             List backups
 -L,--list-files <GUID>                                                List backup files
 -r,--restore <GUID> <NODE_GUID | NODE_NAME> <DIRECTORY>               Restore the backup
 -T,--list-tasks <GUID>                                                List tasks related to the backup
```

## Examples

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

