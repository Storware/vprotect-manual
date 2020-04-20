# Staging space configuration

vProtect Node needs staging space available in `/vprotect_data` by default. It is common to just attach empty drive and mount it.

Staging space size depends on the number and size of simultaneous backups - as a rule of a thumb make it approximately equal to the number of expected simultaneous backup threads multiplied by the size of your biggest VM

**Note**: If you rather want to use vProtect with single **deduplicated space for both staging and backup destination:**

* skip this section **-** so _\*\*_leave `/vprotect_data` folder empty
* create File System Backup destination with deduplication enabled \([File system setup](../initial_config/backup-providers/setup_filesystem.md)\) 
  * vProtect will initialize block device for you.

Otherwise - to have your spare disk to be used as a staging space:

List all existing disks, and find your new disk \(let's say - **/dev/sdc**\):

```text
   [root@vProtect01 ~]# fdisk -l | grep dev
   Disk /dev/sda: 32.2 GB, 32212254720 bytes, 62914560 sectors
   /dev/sda1   *        2048     1026047      512000   83  Linux
   /dev/sda2         1026048    62914559    30944256   8e  Linux LVM
   Disk /dev/sdc: 500 GB, 17179869184 bytes, 33554432 sectors
   Disk /dev/sdb: 21.5 GB, 21474836480 bytes, 41943040 sectors
   Disk /dev/mapper/centos-root: 28.5 GB, 28462546944 bytes, 55590912 sectors
   Disk /dev/mapper/centos-swap: 3221 MB, 3221225472 bytes, 6291456 sectors
```

If you have new clean disk prepare a filesystem on it:

```text
mkfs.xfs -K /dev/sdc
```

Test mount your existing filesystem in created directory:

```text
mount /dev/sdc /vprotect_data
```

Set ownership to vprotect user on directory `/vprotect_data`:

```text
chown vprotect:vprotect -R /vprotect_data
```

Add a line to `/etc/fstab` file, to automatically mount new filesystem after reboot:

```text
/dev/sdc    /vprotect_data    xfs    defaults 0 0
```

Remember to **restart vprotect-node service** to have your staging space discovered properly.

