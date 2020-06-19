# Filesystem

In this section we'll show you how to setup file system \(it can be local or remote file system, but this example assumes that you have a dedicated disk which you're going to use as a backup destination with local XFS file system\)

**Note:**

* Any remote FS like **NFS, SMB, etc.** - needs to be mounted by the user and vprotect user/group must own the directories within backup destination. vProtect expects already mounted file system and mount point in the backup destination
* you should add this file system to your `/etc/fstab` file on the node, so it gets mounted automatically if OS is rebooted
* Consider using same file system for staging and backup destination \(it boosts store tasks, as no data needs to be copied again\) - in such scenario the only difference would be that presented `/backupdestination`mount point becomes subdirectory of the staging space \(usually `/vprotect_data/backups`\) 

## Preparation

1. Log in to vProtect Node, and create mount directory in example `/backupdestination`

   ```text
   mkdir /backupdestination
   ```

2. List all existing disks, and find your drive:

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

3. Prepare filesystem on it:

   ```text
   mkfs.xfs -K /dev/sdc
   ```

4. Add permission for vprotect user to directory `/backupdestination`:

   ```text
   chown vprotect:vprotect -R /backupdestination
   ```

5. Add line to `/etc/fstab` file, to automatically mount new filesystem after reboot:

   ```text
   /dev/sdc    /backupdestination    xfs    defaults 0 0
   ```

   or if you want store backups on NFS share then it will look similar to \(where 10.50.1.28 is your host\):

   ```text
   10.50.1.28:/example_nfs_share /backupdestination nfs defaults  0 0
   ```

6. Check if fstab entry is OK and mount filesystem:

   ```text
   mount /backupdestination
   ```

7. Login to vProtect web UI
8. Go to **Backup Destinations**
9. Click on **Create Backup Destination**, choose **File system**
10. Type name for new backup destination, set retention and select at least one node configuration
11. In **Storage paths** type `/backupdestination`.
12. Save configuration.

