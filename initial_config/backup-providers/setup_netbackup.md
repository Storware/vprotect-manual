# Veritas NetBackup setup

1. Before we start, you have to generate a token for the clients.

   To do that please log in to the Netbackup Administration Console:

   ![](../../.gitbook/assets/setup_netbackup_1.png)

   After successful login please click the “Token Management” submenu under the “Certificate Management”. Next, click the right mouse button on empty space and select “New Token…”

   ![](../../.gitbook/assets/setup_netbackup_2.png)

   Next step is to generate token:

   ![](../../.gitbook/assets/setup_netbackup_3.png)

   Now we should copy the token to clipboard before start the installation of the client software.

   ![](../../.gitbook/assets/setup_netbackup_4.png)

2. To enable Veritas NetBackup support please download and install Veritas NetBackup Client:

   a\) Download the CLIENTS1 package for UNIX clients or the CLIENTS2 package for Linux clients to a system with sufficient space.

   ![](../../.gitbook/assets/setup_netbackup_5.png)

   b\) Extract the contents of the CLIENTS1 or the CLIENTS2 file.

   ```text
   Example:
        AIX         gunzip NetBackup_8.x_CLIENTS1.tar.gz; tar - xvf NetBackup_8.x_CLIENTS1.tar
        HP-UX        gunzip -dc NetBackup_8.x_CLIENTS1.tar.gz | tar -xvf
        Linux        tar -xzvf NetBackup_8.x_CLIENTS2.tar.gz
        Solaris        tar -xzvf NetBackup_8.x_CLIENTS1.tar.gz
   ```

   c\) Change to the directory for your desired operating system.

   ```text
    Example:
        AIX, HP-UX, Solaris, Solaris SPARC    cd [path_to_downloaded_tar.gz file]/NetBackup_8.x_CLIENTS1
        Linux, Linux - s390x            cd [path_to_downloaded_tar.gz file]/NetBackup_8.x_CLIENTS2
   ```

   ![](../../.gitbook/assets/setup_netbackup_6.png)

   d\) Type ./install and answer the questions as follows:

   ![](../../.gitbook/assets/setup_netbackup_7.png)

   When the following message appears, press Enter to continue:
   ![](../../.gitbook/assets/setup_netbackup_8.png)

   The client binaries represent the operating system versions where the binaries were compiled. The binaries typically function perfectly on later versions of the operating system. The installation procedure attempts to load the appropriate binaries for your system. If the script does not recognize the local operating system, it presents choices.

   e\) Type y and press Enter to continue with the software installation.

   ![](../../.gitbook/assets/setup_netbackup_9.png)

   f\) Type the fqdn name of your NetBackup master server \(for examle netbackup.oranization.local for examle netbackup.oranization.local \) and press Enter to continue.

   ![](../../.gitbook/assets/setup_netbackup_10.png)

   ![](../../.gitbook/assets/setup_netbackup_11.png)

   g\) Confirm the NetBackup client name and press Enter to continue.

   ![](../../.gitbook/assets/setup_netbackup_12.png)

   h\) \(Conditional\) Enter one or more media servers if prompted:

   ![](../../.gitbook/assets/setup_netbackup_13.png)

   i\) After you confirm you want to continue, the installer fetches the certificate authority certificate details.

   ![](../../.gitbook/assets/setup_netbackup_14.png)

   Be aware if you press Ctrl+C, this action requires you to rerun the installation or continue with the installation without the required security components. If these security components are absent, backups and restores fail.

   j\) When prompted, review the fingerprint information and confirm that it is accurate.

   ![](../../.gitbook/assets/setup_netbackup_15.png)

   k\) After you confirm the fingerprint information, the installer stores the certificate authority certificate details.

   ![](../../.gitbook/assets/setup_netbackup_16.png)

   Be aware if you press Ctrl+C, this action requires you to rerun the installation or continue with the installation without the required security components. If these security components are absent, backups and restores fail.
   
   l\) After the Certificate Authority certificate is stored, the installer fetches the host certificate.

   ![](../../.gitbook/assets/setup_netbackup_17.png)

   Be aware if you press Ctrl+C, this action requires you to rerun the installation or continue with the installation without the required security components. If these security components are absent, backups and restores fail. 
   
   m\) \(Conditional\) If prompted for the Authorization Token, please enter it.

   ![](../../.gitbook/assets/setup_netbackup_18.png)
   The token format is 16 upper case letters. Be aware if you press Ctrl+C, this action requires you to rerun the installation or continue with the installation without the required security components. If these security components are absent, backups and restores fail. 
   
   n\) This is the last question. Next the setup will make some operations and after all, it will finish its work and exit.

   ![](../../.gitbook/assets/setup_netbackup_19.png)

   o\) Once we have finished the installation of the client software we need to modify the firewall service to allow the double-sided communication between the server and our client. We need to allow input communication on ports 13782/tcp and 13782/udp. To do that enter commands as below:

      * first we need to know what is our default zone: 

         ```text
   firewall-cmd --get-default-zone
         ```
         
         ![](../../.gitbook/assets/setup_netbackup_20.png)
         
         ```text
   firewall-cmd --get-active-zones
         ```
         
         ![](../../.gitbook/assets/setup_netbackup_21.png)

         ```text
   firewall-cmd --list-all
         ```

         ![](../../.gitbook/assets/setup_netbackup_22.png)

      * Like we see, there is no allowing for those ports. So we need to open them.
      
         ```text
   firewall-cmd --zone=public --add-port=13724/udp && firewall-cmd --zone=public --add-port=13724/tcp
         ```
         
         ![](../../.gitbook/assets/setup_netbackup_23.png)

      * Finally we have to modify the permanent firewall rules so that those services will still be available after a reboot.
      
         ```text
   firewall-cmd --zone=public --permanent --add-port=13724/tcp && firewall-cmd --zone=public --permanent --add-port=13724/udp
         ```

         ![](../../.gitbook/assets/setup_netbackup_24.png)

      * Now we can check the status of our firewall:
      
         ```text
   firewall-cmd --zone=public --permanent --list-ports && firewall-cmd --list-all
         ```
         
         ![](../../.gitbook/assets/setup_netbackup_25.png)

      * Last action is to bind the port to the netbackup client deamon” bpnd”. To do that type following command in terminal
         ```text
      /usr/openv/netbackup/bin/bpcd -port 13724
         ```
         
   p\) To add a new client we need to create a policy rule for it where we will configure the type and schedule of this client backup. The client will add automatically after the end of the creator.

   Add new Policy:


   ![](../../.gitbook/assets/setup_netbackup_26.png)

   ![](../../.gitbook/assets/setup_netbackup_27.png)

   ![](../../.gitbook/assets/setup_netbackup_28.png)

   Select type of machine to backup:

   ![](../../.gitbook/assets/setup_netbackup_29.png)

   ![](../../.gitbook/assets/setup_netbackup_30.png)

   Add client to backup:

   ![](../../.gitbook/assets/setup_netbackup_31.png)

   ![](../../.gitbook/assets/setup_netbackup_32.png)

   ![](../../.gitbook/assets/setup_netbackup_33.png)

   Set up configuration details as you want to:

   ![](../../.gitbook/assets/setup_netbackup_34.png) or:

   ![](../../.gitbook/assets/setup_netbackup_35.png)

   ![](../../.gitbook/assets/setup_netbackup_37.png)

   ![](../../.gitbook/assets/setup_netbackup_38.png)

   ![](../../.gitbook/assets/setup_netbackup_39.png)

   ![](../../.gitbook/assets/setup_netbackup_40.png)

   ![](../../.gitbook/assets/setup_netbackup_41.png)

   Finally, we have the client connected to the server.

   ![](../../.gitbook/assets/setup_netbackup_42.png)

3. Allow manual backup for vProtect node client on Netbackup server.
4. Login to vProtect, and go to "Backup Destinations". Click on "Create Backup Destination", chose "Veritas Netbackup". Type name for new backup destination, client home path, real export path. And set Netbackup parameters:

   Client home path

   Real export path

   Policy Name

   Schedule name

   Client name

   ![](../../.gitbook/assets/setup_netbackup_01.png)

