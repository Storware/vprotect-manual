# Amazon EC2 setup

AWS environments require vProtect Node to be installed on one of EC2 instances residing in desired region. It is required to launch instance from **CentOS 7 AMI** and pick instance type with at least 4 GB of memory \(i.e. **t2.medium**\). The VM with vProtect will be automatically detected during index operation.

vProtect Node has access to instances only in the **region where it is hosted**.

vProtect Node requires **account ID,** **access key** and **secret** **key** to connect to AWS account.

## Backup modes

The settings whether Windows or Linux images are required define the way backups and restores are done. AWS supports 2 ways:

1. Using AMI and AWS snapshots:
   * During Export or Snapshot tasks AMI is created for the root volume, other volumes are snapshotted. 
   * AMI is stored in AWS until vProtect snapshot or backup removal is initiated.
   * During Restore a new instance is launched from previously exported AMI and imported non-root volumes are attached.
2. Using AWS snapshots:
   * During Export or Snapshot tasks all volumes are snapshotted.
   * During Restore AMI is created from imported root volume, a new instance is launched and imported non-root volumes are attached. AMI is then removed.

In both cases volume snapshots are kept in AWS only if vProtect Snapshot task is completed.

**Note:** Windows AMI created from snapshot is not launchable, hence it is recommended to **enable using AMI for Windows platform**.

## Restore

It is possible to specify other AMI for attaching non-root volumes during restore. You can also specify an availability zone \(within the region of vProtect Node\) for new instance.

