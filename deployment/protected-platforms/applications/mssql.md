# MSSQL Application
To create a new application for backup MSSQL instances, go to the tab: **Applications -> Instances**

Then select the **Create** button. The Creating an Application Definition section will be displayed, which needs to be completed.

This is a description of how your script is going to be invoked - you need to specify:
* **Name**  - unique name of the application in the vProtect system
* **Choose Node Config** - select which node should perform the task
* **Backup policy** - specify which backup policy should be used for this application
* **Command execution configuration** - select a prepared template for MSSQL backup

In the **Remote PowerShell access** subtab, complete the following fields:
* **Host** - set the address of the host where the MSSQL instance exists
* **Port** - set the host port
* **User** - indicate a user to connect to PS
* **Password** - enter the connection password