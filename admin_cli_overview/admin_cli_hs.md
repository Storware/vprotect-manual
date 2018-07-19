# Hypervisor storage

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

## Examples

* To list all detected storage volumes:

  ```text
  vprotect hs -l
  ```

* To delete a storage volume with GUID `6b5aa45a-5436-47cd-82ce-1c4250742323`:

  ```text
  vprotect hs -d 6b5aa45a-5436-47cd-82ce-1c4250742323
  ```

