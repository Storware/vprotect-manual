# vProtect DB backup

## Build-in vProtect DB automatic backup

It is **highly recommended** to setup periodic **DB backup** on vProtect Server. vProtect provides automatic backups with build-in Application backup. This means that node can create a periodic backups of the database. In order to make your backups being done automatically you need to do the following: 

1. you need to have **node registered** and **at least 1 backup destination configured** 
2. go to **Applications** -&gt; **vProtect DB** application definition
   * select **node**
   * provide **SSH host, user** and **port** - if the setup is on the same node you can provide localhost, root and leave default port 22
   * host corresponds to vProtect server address - you have multiple vProtect environments you can also point to backup one vProtect DB with the other
   * click **Save** \(at the bottom\)
   * update **SSH password** and click **Change Password**
3. Go to **Policies -&gt; Application backup -&gt; vProtect DB Backup Policy** and select **Backup Destination** in the **Rule** section
4. You can now do a test backup of the DB with a backup button \(on the right\) of the DB from your Applications -&gt; vProtect DB list
5. Go to **Schedules -&gt; Application backup -&gt; vProtect DB Backup Schedule** - edit it and change it to active
6. Remember that in case of emergency:
   * if you have a working vProtect - you can use it to restore this file to the specified location and then restore it as described in [Disaster recovery](../admin_dr.md) section
   * if you don't have working vProtect - you can try to use the last copy of the database \(it is left by default in `/tmp/vprotect_db.sql.gz` on the server host.
   * if it is not there and you don't have vProtect working - you may need to use external tools such as S3 browser, TSM client, or just browse through your file system backup destination directories to find and download this file and later restore it as described in [Disaster recovery](../admin_dr.md) section.

Note, that you also may consider creating a separate File System Backup Destination \(which can be local or remote\) just for vProtect DB backup. This would result in easier DB recovery if your vProtect instance is not available.

## Backup with crontab

You also can use the old-style of DB backup setup - using crontab. You just need to setup a cron job to do a periodic backup and transfer it to secure location to protect your configuration from being lost. Please find below commands required to backup and restore vProtect Server database. **Note**: there is no space after `-p`.

* DB backup:

  ```text
  mysqldump -u root -pDBPASSWORD vprotect | gzip -9 > PATH_TO_GZIPPED_BACKUP
  ```

* In case of any issues - you can later restore DB with:

  ```text
  gunzip < PATH_TO_GZIPPED_BACKUP | mysql -u root -pDBPASSWORD vprotect
  ```

