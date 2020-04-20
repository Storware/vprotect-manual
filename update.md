# Update

**Note** that this process only applies for updates from version 3.x. Version 2.x requires a fresh 3.1 installation and database migration.

1. Make sure you have vProtect database backup - instructions how to set it up can be found [here](initial_config/vprotect-db-backup.md)
   * manually you can use this command to backup it on demand on the vProtect Server:`/opt/vprotect/scripts/backup_db.sh /path/to/backup/file.tgz`
2. Make sure you have MariaDB up-to-date - currently vProtect by default uses version **10.4**, while 10.2.31 is the minimum version supported
   * if you need to migrate between versions \(i.e. 10.3 to 10.4\) - we recommend to update it as described [here](https://mariadb.com/kb/en/upgrading-from-mariadb-103-to-mariadb-104) but when you uninstall MariaDB packages you **SHOULD** **NOT** remove vProtect Server package \(as a dependency\) i.e. try `--noautoremove` option: `yum remove --noautoremove MariaDB-server MariaDB-client MariaDB-common`
   * otherwise, minor MariaDB versions should be updated with `yum update`
3. Download new RPM packages `vprotect-server-XXX.rpm` and `vprotect-node-XXX.rpm`
4. Copy Server RPM to host with vProtect Server installed
5. Update Server \(it may take a while, service is being restarted\):

   ```text
   yum -y update vprotect-server-XXX.rpm
   ```

6. Make sure you have updated also the vProtect database \(not just DBMS\), like this \(otherwise vProtect Server may not start\):

   ```text
   mysql_upgrade
   ```

7. If the server service was not running before update you may need to execute also:

   ```text
   systemctl start vprotect-server
   ```

8. Copy Node RPM to all hosts with vProtect Node installed
9. Update each Node:

   ```text
   systemctl stop vprotect-node
   yum -y update vprotect-node-XXX.rpm
   ```

10. If the node service was not running before update you may need to execute also:

    ```text
    systemctl start vprotect-node
    ```

11. Log into the web UI and check if nodes are running.
    * **Note** that you may need to refresh your browser cache after update - for Chrome use `CTRL+SHIFT+R` \(Windows/Linux\) / `CMD+SHIFT+R` \(MacOS\)

