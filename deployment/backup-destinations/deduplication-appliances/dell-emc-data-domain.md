# Dell EMC Data Domain

## Create a new Backup Destination \(Dell EMC Data Domain\)

* Go into the backup destination menu and click on Create a backup destination.
* Provide a name and description for the new backup destination.
* Specify the retention days for full and incremental backups.
* Specify the retention versions for full and incremental backups.
* Choose and assign the node configuration to which you want to attach the new backup destination.
* Add to one or more storage paths.
  * `example - /vprotect_data/backupdestination` 
* Save the configuration.

## DD Boost FS Plugin

**Note:**

* To boost the backup process we recommend using a **single** Storage Unit and mtree, and subfolders on the BoostFS for multiple backup destinations \(with possibly different retention settings\).
  * No additional data copy is needed in the store phase if staging is using the same file system as the backup destination.
  * Setup assumes a single Storage Unit and a single mtree for all backup destinations.
  * The staging space should always be a top directory, and all backup destinations should be defined as separate subfolders of this file system.
  * vProtect handles retention, and each backup destination may have different retention configured.
  * A single Storage Unit will also affect replication as it has to cover all backup destinations, and may replicate temporary data from the staging space or mounted backups.
* **Sharing** the same BoostFS across multiple nodes allows the administrator to create backups on one node \(i.e. one host/environment\) and restore using a different node \(to a different host/environment\).
  * UID/GID ownership and permissions must allow vProtect to read/write contents of the BoostFS share.
  * To meet these requirements, the user and group named vprotect that was created during the installation process must have the same UID and GID on each vProtect node machine. You can create this before installing vProtect packages or change it after installation.

Prepare your PowerProtect DD as a backup destination:

* Login to PowerProtect DD and create an NFS Storage Unit called `storware-vprotect`

![PowerProtect DD - login screen](../../../.gitbook/assets/powerprotect-dd-storage-unit.png)

![DD Boost - storage units](../../../.gitbook/assets/powerprotect-dd-storage-unit2.png)

* Download BoostFS RPM from the Dell EMC site.
* Install BoostFS:

  ```text
  rpm -ivh DDBoostFS-7.0.0.0-633922.rhel.x86_64.rpm
  ```

* Save the password for BoostFS.

  ```text
  # Syntax
  /opt/emc/boostfs/bin/boostfs lockbox set -d [DataDomain_IP_OR_DNS_NAME] -u [Access_User_Name] -s [Storage_Area_Name]
  # Example
  /opt/emc/boostfs/bin/boostfs lockbox set -d 10.1.10.100 -u vprotect -s vprotectbackup
  ```

* Add the /etc/fstab entry:

  ```text
  # Syntax
  [DataDomain_IP_OR_DNS_NAME]:/[Storage_Area_Name] /[Mount_Point] boostfs defaults,_netdev,bfsopt(allow-others=true) 0 0
  # Example
  10.1.10.100:/vprotectbackup /vprotect_data boostfs defaults,_netdev,bfsopt(allow-others=true) 0 0
  ```

* Mount the fstab entry:

  ```text
  mount -a
  ```

* For a manual, one-time mount you can run this command:

  ```text
  # Syntax
  /opt/emc/boostfs/bin/boostfs mount -o allow-others=true -d [DataDomain_IP_OR_DNS_NAME] -s [Storage_Area_Name] /[Mount_Point]
  # Example
  /opt/emc/boostfs/bin/boostfs mount -o allow-others=true -d 10.1.10.100 -s vprotectbackup /vprotect_data
  ```

* Confirm with `df -h` that your `/vprotect_data` is mounted  
  **Note:** Remember to specify the backup destination path as a subdirectory of /vprotect\_data if you would like to use the same storage unit as a staging space and backup destination - for example: /vprotect\_data/my-backups.

  ```text
  mkdir /vprotect_data/my-backups
  ```

* Set ownership to the vprotect user on the directory /vprotect\_data.

    ```
    chown vprotect:vprotect -R /vprotect_data
    ```
*   Set ownership to the vprotect user and data domain group on the directory /vprotect\_data/my-backups.

    ```
    chown vprotect:gid /vprotect_data/my-backups
    ```
where 'gid' is the GID of data domain user specified in Synthetic DD Boost backup destination configuration.
*   Set read and write privileges for both user and group to the directory /vprotect\_data/my-backups.

    ```
    chmod 0775 /vprotect_data/my-backups
    ```

## Synthetic DD Boost

To configure synthetic backup destination, follow the instructions that are described in the [documentation](../filesystem/synthetic-ddboost.md).

