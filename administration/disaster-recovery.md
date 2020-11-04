# Disaster Recovery

## Internal Database Protection

vProtect stores all of the metadata in the local database. It is **highly recommended** to setup periodic DB backup on vProtect Server. Check [Initial configuration](../deployment/initial-configuration.md) for more information.

In case you need to restore vProtect DB:

* if you have a working vProtect - you can use it to restore this file to the specified location and then restore it as described in the [Disaster recovery](disaster-recovery.md) section
* if you don't have working vProtect - you can try to use the last copy of the database \(it is left by default in `/tmp/vprotect_db.sql.gz` on the server host.
* if it is not there and you don't have vProtect working - you may need to use external tools such as S3 browser, TSM client, or just browse through your file system backup destination directories to find and download this file and later restore it as described in [Disaster recovery](disaster-recovery.md) section.

Once you have your backup you can restore DB with the following command:

```text
gunzip < PATH_TO_GZIPPED_BACKUP | mysql -u root -pDBPASSWORD vprotect
```

Additionally, it is recommended to also keep a copy of backup provider-specific files, i.e. `dsm.opt` and `dsm.sys` files for IBM Spectrum Protect, or other config files used by NetBackup, Networker, OpenDedup etc.

In case of a complete loss of the vProtect Server:

1. Reinstall vProtect Server
   * if you lost your license file please contact support
2. Before starting vProtect Server - restore the database
   * you can also restore it later \(i.e. if you want to reinstall it with Ansible or all-in-one option\), but remember to shutdown server first, then restore DB and start the server again 
3. Replace all backup provider-specific files \(install any binaries specific for required backup destinations\)
4. Start vProtect Server service
5. Install vProtect Nodes
6. Make sure the staging path on each node is correct and available
7. Re-register and start nodes

At this point, vProtect should be ready to continue operation.

