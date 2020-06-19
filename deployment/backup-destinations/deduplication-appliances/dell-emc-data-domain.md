# Dell EMC Data Domain

## Create a new Backup Destination \(Dell EMC Data Domain\)

* Go into a backup destination menu and click create backup destination

![](https://github.com/backupmonster/storware-vprotect-manual/tree/31778b5e60e67956cc3fb965d118537bb2d2be7e/.gitbook/assets/backup-destination-add-bd.png)

* Provide a name and description for a new backup destination
* Specify retention days for full and incremental backups
* Specify retention versions for full and incremental backups

![](https://github.com/backupmonster/storware-vprotect-manual/tree/31778b5e60e67956cc3fb965d118537bb2d2be7e/.gitbook/assets/backup-destination-set-retention.png)

* Choose and assign node configuration, to which you want to attach a new backup destination
* Add to one or more storage paths
  * `example - /vprotect_data/backupdestination` 
* Enable/disable deduplication
* Save configuration 

## DD Boost FS Plugin

Prepare your PowerProtect DD as a backup destination:

* Login to PowerProtect DD and create NFS Storage Unit called `storware-vprotect`

![PowerProtect DD Login Screen](https://github.com/backupmonster/storware-vprotect-manual/tree/31778b5e60e67956cc3fb965d118537bb2d2be7e/.gitbook/assets/powerprotect-dd-storage-unit.png)

![DD Boost - Storage Units](https://github.com/backupmonster/storware-vprotect-manual/tree/31778b5e60e67956cc3fb965d118537bb2d2be7e/.gitbook/assets/powerprotect-dd-storage-unit2.png)

* Download BoostFS RPM from Dell EMC site
* Install BoostFS:

  ```text
  rpm -ivh DDBoostFS-7.0.0.0-633922.rhel.x86_64.rpm
  ```

* Mount it on vProtect Node \(`DD_BOOST_IP` should be replaced with IP of your PowerProtect DD\):

  ```text
  /opt/emc/boostfs/bin/boostfs lockbox set -d ip_address -u vprotect -s dell-emc-vprotect
  ```

* Create a directory mkdir /vprotect\_data/backups

  ```text
  /opt/emc/boostfs/bin/boostfs mount -o allow-others=true -d ip_address -s dell-emc-vprotect
  /vprotect_data/backups
  ```

* Confirm with `df` that your `/vprotect_data/backups`

