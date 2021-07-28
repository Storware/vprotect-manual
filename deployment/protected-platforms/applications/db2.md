# DB2 Application

To create a new application for backup DB2 database, go to the tab: **Applications -> Instances**

Then select the **Create** button. The Creating an Application Definition section will be displayed, which needs to be completed.

This is a description of how your script is going to be invoked - you need to specify:

* **Name**  - unique name of the application in the vProtect system
* **Choose Node Config** - select which node should perform the task
* **Backup policy** - specify which backup policy should be used for this application
* **Command execution configuration** - select a prepared template for DB2 backup or create a [new](../../../administration/applications/execution-configurations.md) one yourself

In the SSH access subtab, complete the following fields:

* **Host** - set the address of the host where the MySQL instance exists
* **Port** - set the host port
* **User** - indicate a user to connect to ssh
* **Password** - enter the connection password
* **SSH key path** - alternatively you can specify the ssh key path for authorization

After **saving** the changes to the application, you need to configure the **environment variables** in the **settings section** of your application as needed.

When using the built-in script for database backup, define:

* **VP_DB2_INCREMENTAL** - if backup should be incremental on DB2 server, then please type "Yes". In other case vProtect create full backup
* **VP_DB2_DBLIST** - comma separated list of database names to backup
* **VP_DB2_SCRIPTPATH** - path on DB2 server, where vProtect can place, and execute backup script
* **VP_DB2_PASSWORD** - password to DB2 user database
* **VP_DB2_COMPRESS** - if backup should be compressed on DB2 server, then please type "Yes"
* **VP_DB2_LOGFILE** - path on DB2 server, where vProtect can place the log file from backup job
* **VP_DB2_USER** - login username to DB2
* **VP_DB2_STOREPATH** - path on DB2 server, where vProtect can place backup file

After the new application is fully configured, **save** the changes and go to the [SLA backup configuration](../../../administration/applications/backup-slas.md).