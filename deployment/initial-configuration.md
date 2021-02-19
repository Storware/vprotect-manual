# Initial Configuration

## Node

1. Setup backup destinations \(examples\):
   * [Filesystem](backup-destinations/filesystem/)
   * [Virtual Data Optimizer \(VDO\)](backup-destinations/filesystem/virtual-data-optimizer-vdo.md)
2. For backup strategies involving **disk attachment** mode - follow these steps:[ LVM setup on vProtect Node for disk attachment backup mode](common-tasks/lvm-setup-on-vprotect-node-for-disk-attachment-backup-mode.md).

## Server

1. Upload your license key:
   * if you don't have one, you can contact the Storware team.
   * log in to the web UI and go to the `Settings -> License` and upload your `license.key` file.
2. It is **highly recommended** to setup a vProtect DB backup - the database is key to restore your vProtect environment and later all of the backups that you need.
3. Admin account setup:
   * for audit purposes, it is recommended to add individual admin accounts using `Users` section \(accessible through `Users` menu item\).
   * **Note:** make sure to set the correct **time zone** for each user - the default admin account has **UTC** by default.

## Configuration Wizard

* The configuration wizard can be accessed from the main dashboard by clicking the "configuration wizard" button on the right.

![](../.gitbook/assets/initial-configuration.jpg)

### Welcome page - nodes

* On the welcome page, you should see the vProtect nodes summary. You need at least one fully running node to continue. If you meet this requirement, please click on the next button.

![](../.gitbook/assets/initial-configuration-wizard.jpg)

### Add hypervisor

* In the Hypervisor section, you will start by selecting a hypervisor manager or hypervisor that you want to add. You can repeat this step if you have many types of virtualization providers.

![](../.gitbook/assets/initial-configuration-wizard-hypervisor.jpg)

* For Citrix hypervisor \(as an example\) you have to complete the following parameters

![](../.gitbook/assets/initial-configuration-wizard-hypervisor-example.jpg)

* Choose node

![](../.gitbook/assets/initial-configuration-wizard-hypervisor-example2.jpg)

Select a backup strategy for your hypervisor

![](../.gitbook/assets/initial-configuration-wizard-hypervisor-example3.jpg)

* Optionally you can add an additional NIC for transfer purposes \(provide IP address\)

![](../.gitbook/assets/initial-configuration-wizard-hypervisor-example4.jpg)

* In the end, you will see a popup window that allows you to run inventory synchronization. After that, you should see all virtual machines from that hypervisor.

![](../.gitbook/assets/initial-configuration-wizard-hypervisor-example5.jpg)

### Add backup destination

* In the next section, you can add a backup destination. In this case, you can also repeat the whole process, so you can add multiple providers by the wizard.
* Choose a backup destination \(we use filesystem as an example\)

![](../.gitbook/assets/initial-configuration-wizard-backup-destination.jpg)

* First, enter a name for your backup destination

![](../.gitbook/assets/initial-configuration-wizard-backup-destination-example.jpg)

* You can now customize retention. Each backup destination has its own retention settings. Whichever condition is met first \(either number of versions has been reached or the backup is older than the given limit\), it is removed from the backup destination.

![](../.gitbook/assets/initial-configuration-wizard-backup-destination-example2.jpg)

* Choose, if you want to use deduplication based on [Virtual Data Optimizer \(VDO\)](backup-destinations/filesystem/virtual-data-optimizer-vdo.md)

![](../.gitbook/assets/initial-configuration-wizard-backup-destination-vdo.jpg)

* Setup a storage path, where your data should be stored

![](../.gitbook/assets/initial-configuration-wizard-backup-destination-example4.jpg)

* Optionally you can enable encryption \(AES-256 algorithm\) - if you enable it, remember that you will not benefit from deduplication.

![](../.gitbook/assets/initial-configuration-wizard-backup-destination-encryption.jpg)

* configuration for pre/post execution command. If you use a filesystem with [VDO](backup-destinations/filesystem/virtual-data-optimizer-vdo.md) or [Dell EMC Data Domain](backup-destinations/deduplication-appliances/dell-emc-data-domain.md) integration of standard filesystem, please skip this step. 

![](../.gitbook/assets/initial-configuration-wizard-backup-destination-example6.jpg)

* Decide, if you want to setup this backup destination as a default one. 

![](../.gitbook/assets/initial-configuration-wizard-backup-destination-example7.jpg)

* Finalize a step by going to the next section or add another backup destination 

![](../.gitbook/assets/initial-configuration-wizard-backup-destination-example8.jpg)

### Add schedule

* Choose a name for a schedule and define a type:
  * Full
  * Incremental 

![](../.gitbook/assets/initial-configuration-wizard-schedule-example.jpg)

* Define an execution type:
  * time
  * interval
* Define a start window length 
* Choose time of day for backup

![](../.gitbook/assets/initial-configuration-wizard-schedule-example2.jpg)

* Choose
  * days \(required\). 
  * day of week occurrence \(optionally\)
  * selected months \(optionally\)

![](../.gitbook/assets/initial-configuration-wizard-schedule-example3.jpg)

* Finalize a step by going to the next section or add another schedule 

![](../.gitbook/assets/initial-configuration-wizard-schedule-example4.jpg)

### Add policy

* Choose a name for a policy, auto-remove non-present virtual environments checkbox \(if vProtect should remove VM from the policy that no longer exists\) and priority

![](../.gitbook/assets/initial-configuration-wizard-policy-example.jpg)

* Choose if you want to use auto assign mode based on tags and regular expressions \(matched against VM name, i.e. `.*` matches all characters 0 or more times, check [Policies](https://github.com/backupmonster/dell-emc-vprotect-manual/tree/c077b5308b5ade999cc43068acf4a4f03e38ae58/deployment/administration/virtual-environments/backup-slas/policies.md) section for the details\)

![](../.gitbook/assets/initial-configuration-wizard-policy-example2.jpg)

* Manually add VMs if you do not want to use the auto-assignment mode

![](../.gitbook/assets/initial-configuration-wizard-policy-example3.jpg)

* Choose a backup destination target for this policy

![](../.gitbook/assets/initial-configuration-wizard-policy-example4.jpg)

* Setup a threshold - fail rest of backup tasks if more than X % of EXPORT tasks already failed
* Setup a threshold - fail rest of backup tasks if more than X % of STORE tasks already failed

![](../.gitbook/assets/initial-configuration-wizard-policy-example5.jpg)

* Finalize a step by going to the next section or add another policy 

![](../.gitbook/assets/initial-configuration-wizard-policy-example6.jpg)

### Add internal DB backup

* Choose which node should perform a DB backup

![](../.gitbook/assets/initial-configuration-wizard-database-backup-example.jpg)

* Choose backup destination for DB backup

![](../.gitbook/assets/initial-configuration-wizard-database-backup-example2.jpg)

Choose when a DB backup should be run \(daily basis\)

![](../.gitbook/assets/initial-configuration-wizard-database-backup-example3.jpg)

* Finalize a configuration or run backup manually \(on demand\)

![](../.gitbook/assets/initial-configuration-wizard-database-backup-example4.jpg)

* you are ready to go! 

![](../.gitbook/assets/initial-configuration-wizard-end.jpg)

