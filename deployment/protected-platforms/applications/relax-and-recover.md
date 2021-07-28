# Relax and Recover Application

To create a new application for backup R&R database, go to the tab: **Applications -> Instances**

Then select the **Create** button. The Creating an Application Definition section will be displayed, which needs to be completed.

This is a description of how your script is going to be invoked - you need to specify:

* **Name**  - unique name of the application in the vProtect system
* **Choose Node Config** - select which node should perform the task
* **Backup policy** - specify which backup policy should be used for this application
* **Command execution configuration** - select a prepared template for backup or create a [new](../../../administration/applications/execution-configurations.md) one yourself

In the SSH access subtab, complete the following fields:

* **Host** - set the address of the host where the MySQL instance exists
* **Port** - set the host port
* **User** - indicate a user to connect to ssh
* **Password** - enter the connection password
* **SSH key path** - alternatively you can specify the ssh key path for authorization

After **saving** the changes to the application, you need to configure the **environment variables** in the **settings section** of your application as needed.

When using the built-in script for database backup, define:

* VP_REAR_OUTPUT - Defines where the rescue image should be send to
* VP_REAR_LOGFILE - Path on target server, where vProtect can place the log file from backup job.
* VP_REAR_MOUNTPATH - Path where NFS directory will be mounted
* VP_REAR_STOREPATH - Path on Relax and Recover server, where vProtect can place backup file.
* VP_REAR_METHOD - Relax and Recover backup method
* VP_REAR_RETENTION - Number of days to keep copy
* VP_REAR_NFSSERVER - IP address of the NFS server
* VP_REAR_SCRIPTPATH - Path on target server, where vProtect can place, and execute backup script.

After the new application is fully configured, **save** the changes and go to the [SLA backup configuration](../../../administration/applications/backup-slas.md).