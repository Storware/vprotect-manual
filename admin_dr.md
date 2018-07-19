# Disaster recovery

Storware vProtect stores all of the metadata in the local database. It is **highly recommended** to setup periodic DB backup on vProtect Server. Database should be backed up when no tasks are running and therefore a common practice is to schedule it to be done during the day \(outside VM backup window\). Backup should be transferred to secure location.

Please find below commands required to backup and restore vProtect Server database. **Note**: there is no space after "-p”.

* DB backup:

  ```text
  mysqldump -u root -pDBPASSWORD vprotect | gzip -9 > PATH_TO_GZIPPED_BACKUP
  ```

* In case of any issues - you can later restore DB with:

  ```text
  gunzip < PATH_TO_GZIPPED_BACKUP | mysql -u root -pDBPASSWORD vprotect
  ```

Additionally it is recommend to also keep a copy of backup provider specific files, i.e. `dsm.opt` and `dsm.sys` files for IBM Spectrum Protect, or other config files used by NetBackup, Networker, OpenDedup etc.

In case of a complete loss of the vProtect Server:

1. Reinstall vProtect Server
2. **Before** starting vProtect Server - restore the database
3. Replace all backup provider specific files \(install any binaries specific for required backup destinations\)
4. Start vProtect Server service
5. Install vProtect Nodes
6. Make sure staging path on each node is correct and available
7. Re-register and start nodes

At this point vProtect should be ready to continue operation.

