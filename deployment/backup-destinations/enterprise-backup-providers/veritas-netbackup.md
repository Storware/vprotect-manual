# Veritas Netbackup

* Before we start, you have to generate a token for the clients.
* To do this, please log in to the Netbackup Administration Console:

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-01.png)

* After successful login, please click on the “Token Management” submenu under “Certificate Management”. Next, click the right mouse button on the empty space and select “New Token…”.

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-02%20%281%29.png)

* The next step is to generate a token:

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-03%20%281%29%20%281%29%20%281%29%20%281%29.png)

* Now we must copy the token to the clipboard before we start the installation of the client software.

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-04%20%281%29%20%281%29.png)

* To enable Veritas NetBackup support, please download and install Veritas NetBackup Client:
* Download the CLIENTS1 package for UNIX clients or the CLIENTS2 package for Linux clients to a system with sufficient space.

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-05%20%281%29.png)

* Extract the contents of the CLIENTS1 or the CLIENTS2 file.

```text
Example:
     AIX         gunzip NetBackup_8.x_CLIENTS1.tar.gz; tar - xvf NetBackup_8.x_CLIENTS1.tar
     HP-UX        gunzip -dc NetBackup_8.x_CLIENTS1.tar.gz | tar -xvf
     Linux        tar -xzvf NetBackup_8.x_CLIENTS2.tar.gz
     Solaris        tar -xzvf NetBackup_8.x_CLIENTS1.tar.gz
```

* Change to the directory for your desired operating system.

```text
 Example:
     AIX, HP-UX, Solaris, Solaris SPARC    cd [path_to_downloaded_tar.gz file]/NetBackup_8.x_CLIENTS1
     Linux, Linux - s390x            cd [path_to_downloaded_tar.gz file]/NetBackup_8.x_CLIENTS2
```

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-06%20%281%29%20%282%29%20%282%29%20%282%29%20%282%29%20%282%29.png)

* Type ./install and answer the questions as follows:

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-07.png)

* When the following message appears, press Enter to continue:

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-08%20%281%29.png)

The client binaries represent the operating system versions where the binaries were compiled. The binaries typically function perfectly on later versions of the operating system. The installation procedure attempts to load the appropriate binaries for your system. If the script does not recognize the local operating system, it presents choices.

* Type y and press Enter to continue with the software installation.

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-09%20%281%29.png)

* Type the fqdn name of your NetBackup master server \(for example netbackup.organization.local\) and press Enter to continue.

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-10%20%281%29%20%281%29.png)

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-11.png)

* Confirm the NetBackup client name and press Enter to continue.

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-12%20%281%29.png)

* \(Conditional\) Enter one or more media servers if prompted:

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-13%20%281%29%20%281%29.png)

* After you confirm you want to continue, the installer fetches the authority certificate details.

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-14.png)

**Note:** Be aware that if you press Ctrl+C, this action requires you to rerun the installation or continue with the installation without the required security components. If these security components are absent, backups and restores fail.

* When prompted, review the fingerprint information and confirm that it is accurate.

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-15%20%281%29%20%281%29.png)

* After you have confirmed the fingerprint information, the installer stores the authority certificate details.

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-16%20%281%29.png)

**Note:** Be aware that if you press Ctrl+C, this action requires you to rerun the installation or continue with the installation without the required security components. If these security components are absent, backups and restores fail.

* After the authority certificate is stored, the installer fetches the host certificate.

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-17%20%281%29.png)

**Note:** Be aware that f you press Ctrl+C, this action requires you to rerun the installation or continue with the installation without the required security components. If these security components are absent, backups and restores fail.

* \(Conditional\) If prompted for the Authorization Token, please enter it.

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-18%20%281%29%20%281%29.png)

The token format is 16 upper case letters. Be aware that if you press Ctrl+C, this action requires you to rerun the installation or continue with the installation without the required security components. If these security components are absent, backups and restores fail.

* This is the last question. Next, the setup will carry out some operations, after which it will finish its work and exit.

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-19.png)

* Once we have finished the installation of the client software, we need to modify the firewall service to allow two-way communication between the server and our client. We need to allow input communication on ports 13782/tcp and 13782/udp. To do this, enter the commands as below:
* First, we need to know what our default zone is:

```text
firewall-cmd --get-default-zone
```

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-20%20%281%29.png)

```text
firewall-cmd --get-active-zones
```

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-21.png)

```text
firewall-cmd --list-all
```

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-22.png)

* As we can see, there is no permission for these ports, so we need to open them.

```text
firewall-cmd --zone=public --add-port=13724/udp && firewall-cmd --zone=public --add-port=13724/tcp
```

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-23%20%281%29.png)

* Finally, we have to modify the permanent firewall rules so that the services will still be available after a reboot.

```text
firewall-cmd --zone=public --permanent --add-port=13724/tcp && firewall-cmd --zone=public --permanent --add-port=13724/udp
```

* Now we can check the status of our firewall:

```text
firewall-cmd --zone=public --permanent --list-ports && firewall-cmd --list-all
```

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-25%20%281%29%20%281%29%20%281%29%20%281%29%20%281%29.png)

* The last action is to bind the port to the NetBackup client "deamon bpnd”. To do this, type the following command in the terminal.

```text
/usr/openv/netbackup/bin/bpcd -port 13724
```

* To add a new client, we need to create a policy rule for it where we will configure the type and schedule of this client backup. The client will be added automatically after the creator finishes.

Add a new Policy:

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-26%20%282%29%20%282%29%20%282%29%20%282%29.png)

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-27%20%281%29.png)

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-28%20%281%29.png)

* Select the type of machine to back up:

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-29%20%281%29%20%281%29.png)

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-30%20%281%29.png)

* Add the client to back up:

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-31%20%281%29.png)

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-32.png)

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-33.png)

* Set up the configuration details according to your requirements:

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-34.png)

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-35%20%281%29%20%281%29.png)

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-36%20%281%29%20%281%29%20%281%29%20%281%29%20%281%29.png)

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-37%20%281%29%20%281%29.png)

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-38%20%281%29.png)

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-39%20%281%29.png)

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-40%20%281%29%20%281%29%20%281%29.png)

* We now have the client connected to the server.

![](../../../.gitbook/assets/enterprise-backup-providers-veritas-netbackup-setup-41%20%281%29.png)

* Allow manual backup for the vProtect node client on the Netbackup server.
* Log in to vProtect, go to "Backup Destinations" and select the "enterprise" sub-tab. Click on "Create Backup Destination" and choose "Veritas Netbackup". Type the name for the new backup destination, client home path and real export path. Finally, set the Netbackup parameters:

Client home path  
Real export path  
Policy Name  
Schedule name  
Client name

![](../../../.gitbook/assets/backup-destinations-enterprise-netbackup.jpg)

