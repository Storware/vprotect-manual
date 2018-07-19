# Mounted backups

Mounted backup management module is used to mount and unmounts backups on the given node. This feature is currently supported for RHV/oVirt/OVM VMs. Each mounted backup can be mounted automatically \(auto-detection of mount points within single root or manually with separate mount points for each volume.

To invoke mount/unmount tasks use `vprotect mnt` sub-command.

```text
[root@localhost vprotect]# vprotect mnt
Incorrect syntax: Missing required option: [-T List tasks related to the mounted backup, -u Unmount previously mounted backup, -F List file systems, -l List mounted backups, -L List mounted files, -m Mount backup according to the MOUNT_SPECIFICATION]

usage: mnt -F <GUID> | -l | -L <GUID> | -m <GUID> <NODE_GUID | NODE_NAME> <auto|manual> <MOUNT_SPECIFICATION> | -T <GUID> | -u <GUID>
Mounted backup management
 -F,--list-file-systems <GUID>                                                   List file systems
 -l,--list                                                                       List mounted backups
 -L,--list-files <GUID>                                                          List mounted files
 -m,--mount <GUID> <NODE_GUID | NODE_NAME> <auto|manual> <MOUNT_SPECIFICATION>   Mount backup according to the MOUNT_SPECIFICATION
 -T,--list-tasks <GUID>                                                          List tasks related to the mounted backup
 -u,--unmount <GUID>                                                             Unmount previously mounted backup
```

## Examples

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

