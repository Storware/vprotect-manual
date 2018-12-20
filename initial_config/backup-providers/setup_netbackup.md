# Veritas NetBackup setup

1. To enable Veritas NetBackup support please download and install Veritas NetBackup Client.
   For more information check [Netbackup documentation](https://www.veritas.com/content/support/en_US/doc/27801100-127350444-0/v13833401-127350444).

   
2. Allow manual backup for vProtect node client on Netbackup server.


3. Login to vProtect, and go to "Backup Destinations". Click on "Create Backup Destination", chose "Veritas Netbackup". Type name for new backup destination, client home path, real export path. And set Netbackup parameters:
   Client home path
   Real export path 
   Policy Name
   Schedule name
   Client name

![](../../.gitbook/assets/setup_netbackup_01.png)