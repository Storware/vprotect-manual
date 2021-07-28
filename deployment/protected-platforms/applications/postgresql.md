# PostgreSQL Application

To create a new application for backup PostgreSQL database, go to the tab: **Applications -> Instances**

Then select the **Create** button. The Creating an Application Definition section will be displayed, which needs to be completed.

This is a description of how your script is going to be invoked - you need to specify:

* **Name**  - unique name of the application in the vProtect system
* **Choose Node Config** - select which node should perform the task
* **Backup policy** - specify which backup policy should be used for this application
* **Command execution configuration** - select a prepared template for PostgreSQL backup or create a [new](../../../administration/applications/execution-configurations.md) one yourself

In the SSH access subtab, complete the following fields:

* **Host** - set the address of the host where the MySQL instance exists
* **Port** - set the host port
* **User** - indicate a user to connect to ssh
* **Password** - enter the connection password
* **SSH key path** - alternatively you can specify the ssh key path for authorization

After **saving** the changes to the application, you need to configure the **environment variables** in the **settings section** of your application as needed.

When using the built-in script for database backup, define:

* **VP_SQL_USER** - login username to PostgreSQL
* **VP_SQL_COMPRESS** - if backup should be compressed on PostgreSQL server, then please type "Yes"
* **VP_SQL_PASSWORD** - password to PostgreSQL user database
* **VP_SQL_LOGFILE** - path on PostgreSQL server, where vProtect can place the log file from backup job
* **VP_SQL_DBLIST** - comma separated list of database names to backup
* **VP_SQL_STOREPATH** - path on PostgreSQL server, where vProtect can place temporary backup file. In that path don't store other files, becouse when backup is finished vProtect delete all filres from that path
* **VP_SQL_SCRIPTPATH** - path on PostgreSQL, where vProtect can place, and execute backup script

After the new application is fully configured, **save** the changes and go to the [SLA backup configuration](../../../administration/applications/backup-slas.md).