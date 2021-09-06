# MSSQL

To create a new application for MSSQL instances backup, go to the tab: **Applications -&gt; Instances**

Then select the **Create** button. The Creating an Application Definition section will be displayed, which needs to be completed.

This is a description of how your script is going to be invoked - you need to specify:

* **Name**  - unique name of the application in the vProtect system
* **Choose Node Config** - select which node should perform the task
* **Backup policy** - specify which backup policy should be used for this application
* **Command execution configuration** - select a prepared template for MSSQL backup

On the **Remote PowerShell access** subtab, complete the following fields:

* **Host** - set the address of the host where the MSSQL instance exists
* **Port** - set the host port
* **User** - indicate a user for connecting to the PS
* **Password** - enter the connection password

