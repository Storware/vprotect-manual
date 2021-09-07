# Oracle

To create a new application for backup Oracle database, go to the tab: **Applications -&gt; Instances**

Then select the **Create** button. The Creating an Application Definition section will be displayed, which needs to be completed.

This is a description of how your script is going to be invoked - you need to specify:

* **Name**  - unique name of the application in the vProtect system
* **Choose Node Config** - select which node should perform the task
* **Backup policy** - specify which backup policy should be used for this application
* **Command execution configuration** - select a prepared template for Oracle backup or create a [new](../../../administration/applications/execution-configurations.md) one yourself

In the SSH access subtab, complete the following fields:

* **Host** - set the address of the host where the MySQL instance exists
* **Port** - set the host port
* **User** - indicate a user to connect to ssh
* **Password** - enter the connection password
* **SSH key path** - alternatively you can specify the ssh key path for authorization

After **saving** the changes to the application, you need to configure the **environment variables** in the **settings section** of your application as needed.

When using the built-in script for database backup, define:

* VP\_ORACLE\_DBORARCH - type of backup, allowed is "db" to backup database, "arch" to backup database logs
* VP\_ORACLE\_PASSWORD - password to Oracle database user
* VP\_ORACLE\_RETENTION - how long RMAN should keeps backup. Allowed "RECOVERY\_WINDOWS\_OF\_14\_DAYS" to set number days, "REDUNDANCY\_14" to set number of backups
* VP\_ORACLE\_ENVIROINMENTVARIABLES - path to Oracle database Environment Variables
* VP\_ORACLE\_SCRIPTPATH - path on Oracle server, where vProtect can place, and execute backup script
* VP\_ORACLE\_COMPRESS - if backup should be compressed on Oracle server, then please type "Yes"
* VP\_ORACLE\_USER - login username to Oracle
* VP\_ORACLE\_LOGPATH - path on Oracle server, where vProtect can place the log file from backup job
* VP\_ORACLE\_STOREPATH - path on Oracle server, where vProtect can place backup file

After the new application is fully configured, **save** the changes and go to the [SLA backup configuration](../../../administration/applications/backup-slas.md).

