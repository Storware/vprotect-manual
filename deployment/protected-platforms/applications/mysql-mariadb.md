# MySQL/MariaDB

To create a new application for MySQL database backup, go to the tab: **Applications -&gt; Instances**

Then select the **Create** button. The Creating an Application Definition section will be displayed, which needs to be completed.

This is a description of how your script is going to be invoked - you need to specify:

* **Name**  - unique name of the application in the vProtect system
* **Choose Node Config** - select which node should perform the task
* **Backup policy** - specify which backup policy should be used for this application
* **Command execution configuration** - select a prepared template for MySQL backup or create a [new](../../../administration/applications/execution-configurations.md) one yourself

On the SSH access subtab, complete the following fields:

* **Host** - set the address of the host where the MySQL instance exists
* **Port** - set the host port
* **User** - indicate a user for connecting to the ssh
* **Password** - enter the connection password
* **SSH key path** - alternatively you can specify the ssh key path for authorization

After **saving** the changes to the application, you need to configure the **environment variables** in the **settings section** of your application as needed.

When using the built-in script for database backup, define:

* **VP\_MYSQL\_DBLIST** - a comma separated list of database names to back up
* **VP\_MYSQL\_SCRIPTPATH** - the path to the MySQL/MariaDB server where vProtect can place and execute the backup script
* **VP\_MYSQL\_STOREPATH** - the path to the MySQL/MariaDB server where vProtect can place the temporary backup file. No other files should be stored in the given path since everything is removed after the backup is finished
* **VP\_MYSQL\_USER** - login username to MySQL/MariaDB
* **VP\_MYSQL\_LOGFILE** - the path to the Mysql/MariaDB server where vProtect can place the log file from the backup job
* **VP\_MYSQL\_PASSWORD** - the database user password

After the new application is fully configured, **save** the changes and go to the [SLA backup configuration](../../../administration/applications/backup-slas.md).

