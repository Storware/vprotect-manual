# Build-in application command execution configurations 

vProtect providez set of predefined application command execution configuration. All of them require SSH connection to the server on which the instance being backed up is located. Here is the list of the parameters used in these templates.

## MySQL/MariaDB

- **VP\_MYSQL\_SCRIPTPATH** - Path on MySQL/MariaDB server, where vProtect can place, and execute backup script.

- **VP\_MYSQL\_COMPRESS** - If backup should be compressed on MySQL/MariaDB server, then please type \"Yes\".

- **VP\_MYSQL\_LOGFILE** - Path on MySQL/MariaDB server, where vProtect can place the log file from backup job.

- **VP\_MYSQL\_PASSWORD** - Password to MySQL/MariaDB.

- **VP\_MYSQL\_USER** - Login username to MySQL/MariaDB.

- **VP\_MYSQL\_STOREPATH** - Path on MySQL/MariaDB server, where vProtect can place the temporary backup file. No other files should be stored in the given path since everything is removed after the backup is finished.

- **VP\_MYSQL\_DBLIST** - Comma separated list of database names to backup.



## Oracle DB

- **VP\_ORACLE\_SCRIPTPATH** - Path on Oracle server, where vProtect can place, and execute backup script.
- **VP\_ORACLE\_ENVIROINMENTVARIABLES** - Path to Oracle Database Environment Variables.
- **VP\_ORACLE\_USER** - Login username to Oracle.
- **VP\_ORACLE\_PASSWORD** - Password to Oracle.
- **VP\_ORACLE\_LOGPATH** - Path on Oracle server, where vProtect can place the log file from backup job.
- **VP\_ORACLE\_COMPRESS** - If backup should be compressed on Oracle server, then please type \"Yes\".
- **VP\_ORACLE\_RETENTION** - How long RMAN should keeps backup. Allowed "RECOVERY\_WINDOW\_OF\_14\_DAYS\" to set number of days, \"REDUNDANCY\_14\" to set number of backups.
- **VP\_ORACLE\_DBORARCH** - Type of backup, allowed is \"db\" to backup database, \"arch\" to backup database logs.
- **VP\_ORACLE\_STOREPATH** - Path on Oracle server, where vProtect can place backup file.



## DB2

- **VP\_DB2\_SCRIPTPATH** - Path on DB2 server, where vProtect can place, and execute backup script.
- **VP\_DB2\_USER** - Login username to DB2.
- **VP\_DB2\_PASSWORD** - Password to DB2.
- **VP\_DB2\_LOGFILE** - Path on DB2 server, where vProtect can place the log file from backup job.
- **VP\_DB2\_COMPRESS** - If backup should be compressed on DB2 server, then please type \"Yes\".
- **VP\_DB2\_INCREMENTAL** - If backup should be incremental on DB2 server, then please type \"Yes\". In other case vProtect create full backup.
- **VP\_DB2\_STOREPATH** - Path on DB2 server, where vProtect can place backup file.
- **VP\_DB2\_DBLIST** - Comma separated list of database names to backup.

## PostgreSQL

-  **VP\_SQL\_SCRIPTPATH** -  Path on PosgreSQL server, where vProtect can place, and execute backup script.
- **VP\_SQL\_COMPRESS** - If backup should be compressed on PostgreSQL server, then please type \"Yes\".
- **VP\_SQL\_LOGFILE** - Path on PostgreSQL server, where vProtect can place the log file from backup job.
- **VP\_SQL\_PASSWORD** - Password to PostgreSQL.
- **VP\_SQL\_USER** - Login username to PostreSQL.
- **VP\_SQL\_STOREPATH** - Path on PostgreSQL server, where vProtect can place temporary backup file. In that path don't store other files, because when backup is finished vProtect delete all files from that path.
- **VP\_SQL\_DBLIST** - Comma separated list of database names to backup.

## oVirt/RHV

- **VP\_OVIRT\_LOGFILE** - Path on oVirt/RHV server, where vProtect can place the log file from backup job.



## etcd

- **ETCDCTL\_API** - Etcd API version.
- **VP\_ETCD\_ADDRESS** - Address of etcd instance.
- **VP\_ETCD\_CA\_CERT** - CA cert path.
- **VP\_ETCD\_CERT** - Client cert path.
- **VP\_ETCD\_CLIENT\_KEY** - Client key path.

