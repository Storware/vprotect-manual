# Regular filesystem

In this section, we'll show you how to set up a file system \(it can be a local or remote file system, but this example assumes that you have a dedicated disk which you're going to use as a backup destination with a local XFS file system\)

**Note:**

* Any remote FS like **NFS, SMB, etc.** - needs to be mounted by the user, and the `vprotect` user/group must own the directories within the backup destination. vProtect expects an already mounted file system and mount point in the backup destination.
* You should add this file system to your `/etc/fstab` file on the node, so that it gets mounted automatically if the OS is rebooted.
* Consider using the same file system for the staging and backup destination \(this boosts storage tasks, as no data needs to be copied again\) - in such a scenario, the only difference would be that the presented `/backupdestination`mount point becomes a subdirectory of the staging space \(usually `/vprotect_data/backups`\) .

## Preparation

1. Log in to vProtect Node and create the mount directory as in the example `/backupdestination`

   ```text
   mkdir /backupdestination
   ```

2. List all existing disks and find your drive:

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

3. Prepare a filesystem on it:

   ```text
   mkfs.xfs -K /dev/sdc
   ```

4. Add permission for the vprotect user to access the directory `/backupdestination`

   * we assume here that you use a separate file system than your staging space
   * as an alternative you also can point vProtect to use a subdirectory on the same file system as your staging space, i.e. `/vprotect_data/backups` \(which you probably don't have to initialize at this point, as you may have already prepared it in the [Staging space configuration](../../common-tasks/staging-space-configuration.md), and you can just jump to the Web UI part in the next steps\). 

   ```text
   chown vprotect:vprotect -R /backupdestination
   ```

5. Add this line to the `/etc/fstab` file to automatically mount new the filesystem after reboot:

   ```text
   /dev/sdc    /backupdestination    xfs    defaults 0 0
   ```

   or if you want store backups on NFS share then it will look like this \(where 10.50.1.28 is your host\):

   ```text
   10.50.1.28:/example_nfs_share /backupdestination nfs defaults  0 0
   ```

6. Check if the fstab entry is OK and mount the filesystem:

   ```text
   mount /backupdestination
   ```

7. Log in to the vProtect web UI.
8. Go to **Backup Destinations.**
9. Click on **Create Backup Destination**, choose **File system.**
10. Type the name for the new backup destination, set the retention and select at least one node configuration.
11. Usually you have to decide if your backup destination is a separate entity than the staging space.
    * If the **staging space is different than your backup storage destination:**
      * In **Storage paths** type `/backupdestination` - this path will be used to mount the prepared file system \(XFS\) on top of the VDO volume.
    * If the **staging space needs to be the same as your backup storage destination:**
      * In **Storage paths** type `/vprotect_data/backups`, where you point to a subdirectory \(i.e. `backups` on your staging space path, i.e. `/vprotect_data).`
12. Save the configuration.
