# PostgreSQL

To create a new application for PostgreSQL database backup, go to the tab: **Applications -&gt; Instances**

Then select the **Create** button. The Creating an Application Definition section will be displayed, which needs to be completed.

This is a description of how your script is going to be invoked - you need to specify:

* **Name**  - unique name of the application in the vProtect system
* **Choose Node Config** - select which node should perform the task
* **Backup policy** - specify which backup policy should be used for this application
* **Command execution configuration** - select a prepared template for PostgreSQL backup or create a [new](../../../administration/applications/execution-configurations.md) one yourself

In the SSH access subtab, complete the following fields:

* **Host** - set the address of the host where the MySQL instance exists
* **Port** - set the host port
* **User** - indicate a user for connecting to the ssh
* **Password** - enter the connection password
* **SSH key path** - alternatively you can specify the ssh key path for authorization

After **saving** the changes to the application, you need to configure the **environment variables** in the **settings section** of your application as needed.

When using the built-in script for database backup, define:

* **VP\_SQL\_USER** - login username to PostgreSQL
* **VP\_SQL\_COMPRESS** - if backup should be compressed on PostgreSQL server, then please type "Yes"
* **VP\_SQL\_PASSWORD** - password to PostgreSQL user database
* **VP\_SQL\_LOGFILE** - path to PostgreSQL server where vProtect can place the log file from the backup job
* **VP\_SQL\_DBLIST** - a comma separated list of database names to back up
* **VP\_SQL\_STOREPATH** - the path to the PostgreSQL server where vProtect can place the temporary backup file. Do not store other files in this path because when backup is finished, vProtect deletes all files from the path
* **VP\_SQL\_SCRIPTPATH** - the path to the PostgreSQL where vProtect can place and execute the backup script

After the new application is fully configured, **save** the changes and go to the [SLA backup configuration](../../../administration/applications/backup-slas.md).

