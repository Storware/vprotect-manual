# Relax and Recover

To create a new application for R&R database backup, go to the tab: **Applications -&gt; Instances**

Then select the **Create** button. The Creating an Application Definition section will be displayed, which needs to be completed.

This is a description of how your script is going to be invoked - you need to specify:

* **Name**  - unique name of the application in the vProtect system
* **Choose Node Config** - select which node should perform the task
* **Backup policy** - specify which backup policy should be used for this application
* **Command execution configuration** - select a prepared template for backup or create a [new](../../../administration/applications/execution-configurations.md) one yourself

In the SSH access subtab, complete the following fields:

* **Host** - set the address of the host where the MySQL instance exists
* **Port** - set the host port
* **User** - indicate a user for connecting to the ssh
* **Password** - enter the connection password
* **SSH key path** - alternatively you can specify the ssh key path for authorization

After **saving** the changes to the application, you need to configure the **environment variables** in the **settings section** of your application as needed.

When using the built-in script for database backup, define:

* VP\_REAR\_OUTPUT - Defines where the rescue image should be sent
* VP\_REAR\_LOGFILE - Path to the target server, where vProtect can place the log file from the backup job.
* VP\_REAR\_MOUNTPATH - Path where the NFS directory will be mounted
* VP\_REAR\_STOREPATH - Path to the Relax and Recover server, where vProtect can place the backup file.
* VP\_REAR\_METHOD - Relax and Recover backup method
* VP\_REAR\_RETENTION - Number of days to keep the copy
* VP\_REAR\_NFSSERVER - IP address of the NFS server
* VP\_REAR\_SCRIPTPATH - Path to the target server, where vProtect can place and execute the backup script.

After the new application is fully configured, **save** the changes and go to the [SLA backup configuration](../../../administration/applications/backup-slas.md).
