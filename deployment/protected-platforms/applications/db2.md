# DB2

To create a new application for DB2 database backup, go to the tab: **Applications -&gt; Instances**

Then select the **Create** button. The Creating an Application Definition section will be displayed, which needs to be completed.

This is a description of how your script is going to be invoked - you need to specify:

* **Name**  - unique name of the application in the vProtect system
* **Choose Node Config** - select which node should perform the task
* **Backup policy** - specify which backup policy should be used for this application
* **Command execution configuration** - select a prepared template for DB2 backup or create a [new](../../../administration/applications/execution-configurations.md) one yourself

In the SSH access subtab, complete the following fields:

* **Host** - set the address of the host where the MySQL instance exists
* **Port** - set the host port
* **User** - indicate a user for connecting to the ssh
* **Password** - enter the connection password
* **SSH key path** - alternatively you can specify the ssh key path for authorization

After **saving** the changes to the application, you need to configure the **environment variables** in the **settings section** of your application as needed.

When using the built-in script for database backup, define:

* **VP\_DB2\_INCREMENTAL** - if backup should be incremental on the DB2 server, then please type "Yes". Otherwise, vProtect will create a full backup
* **VP\_DB2\_DBLIST** - a comma separated list of database names to back up
* **VP\_DB2\_SCRIPTPATH** - the path to the DB2 server where vProtect can place and execute the backup script
* **VP\_DB2\_PASSWORD** - password to the DB2 user database
* **VP\_DB2\_COMPRESS** - if backup should be compressed on the DB2 server, then please type "Yes"
* **VP\_DB2\_LOGFILE** - path to the DB2 server where vProtect can place the log file from the backup job
* **VP\_DB2\_USER** - login username to DB2
* **VP\_DB2\_STOREPATH** - the path to the DB2 server where vProtect can place the backup file

After the new application is fully configured, **save** the changes and go to the [SLA backup configuration](../../../administration/applications/backup-slas.md).

