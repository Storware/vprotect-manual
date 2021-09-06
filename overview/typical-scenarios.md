# Typical Scenarios

## Backup & Recovery

The core functionality of vProtect is **agent-less backup** for multiple virtualization, container and cloud platforms.

With snapshot-based backups you don't have to install any agent inside VMs or customize your hypervisors.

Backups performed by vProtect usually are crash-consistent, but you can enable **application-consistency** or enhance backup process with your **custom pre/post snapshot remote command execution**.

Snapshots are exported from your virtualization platform and can be stored in backup provider of your choice. You can use enterprise-grade backup providers, object storage or just file system as your target.

This means that vProtect can act as a **stand-alone solution** or a **proxy to your existing storage or enterprise backup provider.**

You also can periodically restore your VMs to verify if your backups are consistent.

With mounted backups - you also can **restore individual files** from your backups via Web UI or directly from vProtect Node.

## Disaster Recovery

A real disaster can sometimes happen - with vProtect you can configure your backups to be performed in one datacenter and - if necessary - restore them in the second one.

vProtect can use replicated file systems or other build-in mechanisms of backup providers to allow you to keep a copy in the secondary datacenter.

During DR, you can use Recovery Plans to restore multiple VMs to the predefined location.

## Snapshot Management

Backups are usually quite intensive operation. Snapshots have to be exported and stored, which usually means that you can't perform them too often. With vProtect you can use Snapshot Management policies to periodically create additional snapshots on your VMs without the need to export them.

When you need to restore a VM to the most recent saved state, you can quickly revert snapshot that vProtect has created for you.

## Application Backup & Recovery

There are many cases where VM-level backup may not be enough. Applications, such as databases usually have their own mechanisms that guarantee consistent backups. As we are aware that in many situations you need to have the option to customize the backup process - vProtect provides a **generic mechanism** for multiple scenarios.

With vProtect, you can prepare a custom script or invoke any backup command that produces backup artefacts \(or just initiates external backup process\) on a remote host and store backups to your backup provider.

With Application backup, you can extend your protection capabilities to:

* any remote  applications with  their own mechanisms
* hypervisor configuration
* files on remote hosts \(physical, virtual or containers\)
  * this includes shares, mounted object-storage buckets, LVM block devices or virtually anything which can be presented as a file
* initiating external backup processes such as RMAN

