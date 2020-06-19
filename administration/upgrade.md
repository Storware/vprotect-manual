# Upgrade

Before every update check installed packages version. Database version is particularly important.

```text
yum info vprotect-server vprotect-node mariadb-server
# Or
rpm -qa | egrep -e "vprotect|Maria"
```

If the host computer has an Internet connection, use the yum command, you'll also see the new package versions provided by the repositories.

## Server Upgrade

**Note** that this process only applies for updates from version 3.x. Version 2.x requires a fresh 3.1 installation and database migration.

* Make sure you have vProtect database backup.
  * manually you can use this command to backup it on demand on the vProtect Server:`/opt/vprotect/scripts/backup_db.sh /path/to/backup/file.tgz`
* If vProtect was installed on a virtual machine \(not a physical one\), it would be a good move to take a snapshot
* After backing up the database, we should gently stop the vProtect service to make sure that we don't have any tasks running \(running task may cause problems updating the database\)
  * View all tasks, if you see at least one on the list, clear it \(wait for the ongoing tasks to finish\)
  * You can do this from WebUI \(it's faster\)

```text
[root@vprotect ~]# vprotect task -L          

                GUID                   Type    State    [%]    Window start       Window end     Pri.     Node       VM/APP     
------------------------------------  ------  --------  ---  ----------------  ----------------  ----  ----------  -----------  
e3bb2496-3928-417c-a604-8c61b64df90e  Export  Running   0    2020-06-19 12:27  2020-06-19 17:27  50    vPro-Local  VM_01_Apine  
05c1d6cc-fe3b-40fb-9811-94b976571d8e  Store   Finished  100  2020-06-19 12:10  2020-06-19 17:10  50    vPro-Local  VM_01_Apine  
cb47190d-cf10-4cf9-8d1d-418eed5accf9  Export  Finished  100  2020-06-19 12:09  2020-06-19 17:09  50    vPro-Local  VM_01_Apine

#To delete a task from the list
[root@vprotect ~]# vprotect task -d cb47190d-cf10-4cf9-8d1d-418eed5accf9
```

* Now when you don't have any tasks on the list, you can stop the service

```text
[root@vprotect ~]# systemctl disable vprotect-server --now
```

* To make sure that no scheduler has started the task before stopping the service, let's query the database
  * If the table is not empty, start the vProtect-Server service and clear the tasks again

```text
mysql -u root -p -e "Select * FROM vprotect.task;"
```

* Make sure you have MariaDB up-to-date - currently vProtect by default uses version **10.4**, while 10.2.31 is the minimum version supported
  * if you need to migrate between versions \(i.e. 10.3 to 10.4\) - we recommend to update it as described [here](https://mariadb.com/kb/en/upgrading-from-mariadb-103-to-mariadb-104) but when you uninstall MariaDB packages you **SHOULD** **NOT** remove vProtect Server package \(as a dependency\) i.e. try `--noautoremove` option:  _As centos/rhel 7 do not have the --noautoremove option natively, please use rpm method._
  * otherwise, minor MariaDB versions should be updated with `yum update`
  * `rpm -e --nodeps "MariaDB-server-YOUR_VERSION_OF_PACKAGE"`
  * Update MariaDB repository to correct version `vi /etc/yum.repos.d/MariaDB.repo`
  * Install new MariaDB-Server `yum install -y mariadb-server`
  * Update all other components of MariaDB `yum update -y mariadb`
  * Start MariaDB engine `systemctl enable mariadb --now`
  * Run mysql\_upgrade to update vProtect Database `mysql_upgrade --user=root --password`
* If the database update is successful, now we can start with vProtect Update. Make sure you configure our new repository for vProtect - new base url:  
  [http://repo.storware.eu/vprotect/current/el8](http://repo.storware.eu/vprotect/current/el8) or [http://repo.storware.eu/vprotect/current/el7](http://repo.storware.eu/vprotect/current/el7)

  `vi /etc/yum.repos.d/vProtect.repo`

* Update Server \(it may take a while, service is being restarted\):

```text
yum -y update vprotect-server
```

* If the server service was not running before update you may need to execute also:

  ```text
  systemctl enable vprotect-server --now
  ```

## Node Upgrade

1. Copy Node RPM to all hosts with vProtect Node installed
2. Update each Node:

   ```text
   systemctl stop vprotect-node
   yum -y update vprotect-node
   ```

3. If the node service was not running before update you may need to execute also:

   ```text
   systemctl enable vprotect-node --now
   ```

4. Log into the web UI and check if nodes are running.
   * **Note** that you may need to refresh your browser cache after update - for Chrome use `CTRL+SHIFT+R` \(Windows/Linux\) / `CMD+SHIFT+R` \(MacOS\)

