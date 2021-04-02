# File-level Restore \(Mounted Backup\)

**Note:** To see which hypervisor support this feature please go to [vProtect Support Matrix](https://storware.gitbook.io/storware-vprotect/planning/vprotect-support-matrix).

To mount backup go to the Instances tab under Virtual Environment section on the left side menu, then click on the mount icon next to a chosen virtual machine ![](../../.gitbook/assets/icon-mount.jpg)

![](../../.gitbook/assets/instances%20%282%29%20%283%29%20%283%29.jpg)

On the popup window, you can select which backup you want to mount and on which node. You can also change the mount method, but we recommend leaving the default setting "Mount filesystem automatically".

![](../../.gitbook/assets/file-level-restore%20%281%29.jpg)

The Mounted Backups tab show mounted Virtual Machine backup's on the vProtect node.

![](../../.gitbook/assets/file-level-restore-list.jpg)

* `VIRTUAL MACHINE` - mounted virtual machine name
* `MODE` - Auto - vProtect auto-detect filesystems and mount it on path "/mnt/vprotect/". In Manual mode user chose mount point for selected filesystems.
* `NODE` - vProtect node responsible for mount job.
* `SNAPSHOT DATE` - date of mounted backup of the VM.
* `FILE SYSTEMS` - number of mounted filesystems.
* `FILES` - number of mounted virtual disk images.

Next to every mounted backup you can see three buttons:  
To unmount backup click on ![](../../.gitbook/assets/icon-unmount%20%282%29%20%282%29.jpg)  
To remount backup click on ![](../../.gitbook/assets/icon-remount%20%281%29.jpg)  
To go to the details page of mounted backup click on ![](../../.gitbook/assets/icon-magnifier%20%281%29%20%281%29.jpg)

On the details page, you can view some basic information or go deeper and browse files.

![](../../.gitbook/assets/file-level-restore-details-page.jpg)

With a web browser, you can obtain even a single file from inside of your virtual machine backup.

![](../../.gitbook/assets/file-level-restore-browse.jpg)

## You can also perform the same action thanks to the CLI interface: [CLI Reference](file-level-restore-mounted-backup-1.md)

