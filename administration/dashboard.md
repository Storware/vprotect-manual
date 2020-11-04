# Dashboard

![](../.gitbook/assets/dashboard-overview%20%281%29.jpg)

## Overview

The primary vProtect interface is the WEB UI accessibly via web browser.

Divided into a few sections, gives the ability to view and set the most vital options related to management, monitoring and reporting.

Left pane contains the main menu \(see below for a more detailed description\).

Upper right corner provides access to documentation, support and system logs.

Bottom part gives you the ability to look at the \(sliding\) task console.

Menu on the left provides access to the most important sections:

* `Dashboard` - main screen with general summary and configuration wizard
* `Nodes` - node management and node configurations
* `Virtual Environments`
  * `Instances` - list of currently known virtual machines and access to the details page of each object
  * `Infrastructure` - access configuration for hypervisors and hypervisors managers, basic info about the inventoried environment like clusters and storage
  * `Backup SLAs` - allow you to setup a correlation between virtual machines, backup destinations and schedules \(Policies tab\). It also allows you to configure policy schedules \(schedules tab\).
  * `Snapshot SLAs` - allow you to setup a correlation between virtual machines, snapshot retention and schedules \(Policies tab\). It also allows you to configure policy schedules \(schedules tab\).
  * `Recovery Plans` - allow you to setup a number of rules to restore your virtual machines into hypervisor or filesystem to test your backups \(Policies tab\). It also allows you to configure policy schedules \(schedules tab\).
  * `Mounted Backups` - browse and download files from mounted backups
* `Applications`
  * `Instances`- Create Application definition \(at least its name and Command Execution Configuration and select node which is going to do the work\)
  * `Execution Configurations` - Prepare script or commands \(this is a description of how your script is going to be invoked\)
  * `Backup SLAs` - allow you to setup a correlation between applications, backup destinations and schedules. It also allows you to configure policy schedules \(schedules tab\).
* `Reporting`
  * `Virtual environments` - backup and restore statistics for virtual environments
  * `Applications` - backup and restore statistics for defined applications
  * `Audit Log` - watch what actions took place in the application
* `Backup Destinations` - create and manage backup destinations \(including backup retention\)
  * `File system` - You can use Regular filesystem, remote \(NFS, SMB, etc.\) or attach a block device and enable Virtual Data Optimizer \(VDO\).
  * `Object storage` - on-premise or cloud-based vendors of object-oriented storage, allowing for safe storage of backups \(AWS, Azure, GCS, Ceph, Scality etc.\)
  * `Enterprise` -  configure integration with enterprise backup providers as your backup destination \(Dell EMC, IBM, Veritas\)
* `Users` - create and manage your accounts \(Language, time zones etc.\)
* `Settings` - From here, you can manage global settings, licenses, email, authentication and internal DB backup

