# Typical Scenarios

## Backup & Recovery

The core functionality of vProtect is **agent-less backup** for multiple virtualization, container and cloud platforms.

With snapshot-based backups you don't have to install an agent inside VMs or customize your hypervisors.

Backups performed by vProtect usually are crash-consistent, but you can enable **application-consistency** or enhance the backup process with your own **custom pre/post snapshot remote command execution**.

Snapshots are exported from your virtualization platform and can be stored in the backup provider of your choice. You can use enterprise-grade backup providers, object storage or just a file system as your target.

This means that vProtect can act as a **stand-alone solution** or as a **proxy to your existing storage or enterprise backup provider.**

You also can periodically restore your VMs to verify if your backups are consistent.

With mounted backups you can also **restore individual files** from your backups via a Web UI or directly from vProtect Node.

## Disaster Recovery

Real disasters can sometimes happen - with vProtect you can configure your backups to be performed in one datacenter and - if necessary - restore them in a second datacenter.

vProtect can use replicated file systems or other built-in backup provider mechanisms to allow you to keep a copy in the secondary datacenter.

During DR, you can use Recovery Plans to restore multiple VMs to a predefined location.

## Snapshot Management

Backups are usually quite an intensive operation. Snapshots have to be exported and stored, which usually means that you can't perform them too often. With vProtect you can use Snapshot Management policies to periodically create additional snapshots on your VMs without the need to export them.

When you need to restore a VM to the most recent saved state, you can quickly revert to a snapshot that vProtect has created for you.

## Application Backup & Recovery

There are many cases where VM-level backup may not be enough. Applications such as databases usually have their own mechanisms that guarantee consistent backups. As we are aware, in many situations you need to have the option to customize the backup process - therefore vProtect provides a **generic mechanism** for multiple scenarios.

With vProtect, you can prepare a custom script or invoke any backup command that produces backup artefacts \(or just initiates the external backup process\) on a remote host and stores backups to your backup provider.

With Application backup, you can extend your protection capabilities to:

* any remote applications with their own mechanisms
* hypervisor configuration
* files on remote hosts \(physical, virtual or containers\)
  * this includes shares, mounted object-storage buckets, LVM block devices or virtually anything which can be presented as a file
* initiating external backup processes such as RMAN

