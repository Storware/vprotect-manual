# Initial Configuration

## Node

1. Setup backup destinations \(examples\):
   * [Filesystem](backup-destinations/filesystem/)
   * [Virtual Data Optimizer \(VDO\)](backup-destinations/filesystem/virtual-data-optimizer-vdo.md)
2. For backup strategies involving **disk attachment** mode - follow these steps:[ LVM setup on vProtect Node for disk attachment backup mode](common-tasks/lvm-setup-on-vprotect-node-for-disk-attachment-backup-mode.md).

## Server

1. Upload your license key:
   * if you don't have it you can request to Storware team.
   * log in to the web UI and go to the `Settings -> License` and upload your `license.key` file.
2. It is **highly recommended** to setup vProtect DB backup - database is key to restore your vProtect environment and later all of the backups that you need.
3. Admin account setup:
   * for audit purposes it is recommended to add individual admin accounts using `Users` section \(accessible through `Users` menu item\).
   * **Note:** make sure to set the correct **time zone** for each user - default admin account has **UTC** by default.

## Configuration Wizard

* The configuration wizard can be accessed from the main dashboard by clicking the "configuration wizard" button on the right.

![](../.gitbook/assets/configuration-wizard-00.png)

### Welcome page - nodes

* At the welcome page you should see vProtect nodes summary. You need at least one fully running node to continue. If you meet this requirement, please click on next button.

![](../.gitbook/assets/configuration-wizard-01%20%281%29.jpg)

### Add hypervisor

* In Hypervisor section you will start from selecting hypervisor manager or hypervisor that you want to add. You can repeat this step if you have many types of virtualization providers.

![](../.gitbook/assets/configuration-wizard-01.jpg)

* For Citrix hypervisor \(as an example\) you have to fill below parameters

![](../.gitbook/assets/configuration-wizard-02.jpg)

* Choose node

![](../.gitbook/assets/configuration-wizard-03.jpg)

Select backup strategy for your hypervisor

![](../.gitbook/assets/configuration-wizard-04.jpg)

* Optionally you can add an additional NIC for transfer purposes

![](../.gitbook/assets/configuration-wizard-05.jpg)

* At the end, you will see popup window that allows you to run inventory synchronization. After that you should see all virtual machines from that hypervisor.

![](../.gitbook/assets/configuration-wizard-06.jpg)

### Add backup destination

* In the next section you can add a backup destination. In this case you can also repeat the whole process, so you can add multiple providers by the wizard.
* Choose a backup destination \(we use filesystem as an example\)

![](../.gitbook/assets/configuration-wizard-7_1.png)

* First, enter a name for your backup destination

![](../.gitbook/assets/configuration-wizard-07.jpg)

* You can now customize retention. Each backup destination has its own retention settings. Whichever condition is met first \(either number of versions has been reached or backup is older than given limit\), it is removed from backup destination.

![](../.gitbook/assets/configuration-wizard-08.jpg)

* Choose, if you want to use deduplication based on [Virtual Data Optimizer \(VDO\)](backup-destinations/filesystem/virtual-data-optimizer-vdo.md)

![](../.gitbook/assets/configuration-wizard-8_1.png)

* Setup a storage path, where your data should be stored

![](../.gitbook/assets/configuration-wizard-09.jpg)

* Optionally you can enable encryption \(AES-256 algorithm\)

![](../.gitbook/assets/configuration-wizard-10.jpg)

* configuration for pre/post execution command. If you use filesystem with [VDO](backup-destinations/filesystem/virtual-data-optimizer-vdo.md) or [Dell EMC Data Domain](backup-destinations/deduplication-appliances/dell-emc-data-domain.md) integration of standard filesystem, please skip this step. 

![](../.gitbook/assets/configuration-wizard-11.jpg)

* Decide, if you want to setup this backup destination as a default one. 

![](../.gitbook/assets/configuration-wizard-12.jpg)

* Finalize a step by going to a next section or  add another backup destination 

![](../.gitbook/assets/configuration-wizard-13.jpg)

### Add schedule

* Choose a name for a schedule and define a type:
  * Full
  * Incremental 

![](../.gitbook/assets/configuration-wizard-14.jpg)

* Define a execution type:
  * time
  * interval
* Define a start window length 
* Choose time of day for backup

![](../.gitbook/assets/configuration-wizard-15.jpg)

* Choose 
  * days \(required\). 
  * day of week occurrence \(optionally\)
  * selected months \(optionally\)

![](../.gitbook/assets/configuration-wizard-16.jpg)

* Finalize a step by going to a next section or  add another schedule 

![](../.gitbook/assets/configuration-wizard-17.jpg)

### Add policy

* Choose a name for a policy, auto remove non-present virtual environments checkbox and priority

![](../.gitbook/assets/configuration-wizard-18.jpg)

* Choose if you want to use auto assign mode based on tags and regular expressions

![](../.gitbook/assets/configuration-wizard-19.jpg)

* Manually add VMs

![](../.gitbook/assets/configuration-wizard-20.jpg)

* Choose a backup destination target for this policy

![](../.gitbook/assets/configuration-wizard-21.png)

* Setup a threshold - fail rest of backup tasks if more than X % of EXPORT tasks already failed
* Setup a threshold - fail rest of backup tasks if more than X % of STORE tasks already failed

![](../.gitbook/assets/configuration-wizard-22.jpg)

* Finalize a step by going to a next section or  add another policy 

![](../.gitbook/assets/configuration-wizard-23.jpg)

### Add internal DB backup

* Choose which node should perform a DB backup

![](../.gitbook/assets/configuration-wizard-24.jpg)

* Choose backup destination for DB backup

![](../.gitbook/assets/configuration-wizard-25.jpg)

Choose when a DB backup should be run \(daily basis\)

![](../.gitbook/assets/configuration-wizard-26.jpg)

* Finalize a configuration or run backup manually \(on demand\)

![](../.gitbook/assets/configuration-wizard-27.jpg)

* you are ready to go! 

![](../.gitbook/assets/configuration-wizard-28.png)

