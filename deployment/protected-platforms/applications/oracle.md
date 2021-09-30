# Oracle

To create a new application for Oracle database backup, go to the tab: **Applications -&gt; Instances**

Then select the **Create** button. The Creating an Application Definition section will be displayed, which needs to be completed.

This is a description of how your script is going to be invoked - you need to specify:

* **Name**  - unique name of the application in the vProtect system
* **Choose Node Config** - select which node should perform the task
* **Backup policy** - specify which backup policy should be used for this application
* **Command execution configuration** - select a prepared template for Oracle backup or create a [new](../../../administration/applications/execution-configurations.md) one yourself

In the SSH access subtab, complete the following fields:

* **Host** - set the address of the host where the MySQL instance exists
* **Port** - set the host port
* **User** - indicate a user for connecting to the ssh
* **Password** - enter the connection password
* **SSH key path** - alternatively you can specify the ssh key path for authorization

After **saving** the changes to the application, you need to configure the **environment variables** in the **settings section** of your application as needed.

When using the built-in script for database backup, define:

* VP\_ORACLE\_DBORARCH - type of backup, either "db" to back up the database, or "arch" to back up the database logs
* VP\_ORACLE\_PASSWORD - Oracle database user password
* VP\_ORACLE\_RETENTION - how long RMAN should keep backup. Allowed "RECOVERY\_WINDOWS\_OF\_14\_DAYS" to set the number of days, "REDUNDANCY\_14" to set the number of backups
* VP\_ORACLE\_ENVIROINMENTVARIABLES - path to the Oracle database Environment Variables
* VP\_ORACLE\_SCRIPTPATH - path to the Oracle server, where vProtect can place and execute the backup script
* VP\_ORACLE\_COMPRESS - if backup should be compressed on the Oracle server, then please type "Yes"
* VP\_ORACLE\_USER - login username for Oracle
* VP\_ORACLE\_LOGPATH - path to the Oracle server, where vProtect can place the log file from the backup job
* VP\_ORACLE\_STOREPATH - path to the Oracle server, where vProtect can place the backup file

After the new application is fully configured, **save** the changes and go to the [SLA backup configuration](../../../administration/applications/backup-slas.md).

