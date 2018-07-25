# Settings

`Settings` section contains all global settings \(for all nodes and UI\):

## Global

* `Node status update interval` - how often nodes should update their status
* `Backup history retention` - how long should the history of backups be kept \(even removed from backup provider\)
* `Task retention (in console)` - how long finished/failed tasks should be kept in the console in UI/CLI
* `Periodic index interval` - how often vProtect should scan for changes in VM inventory on HV/HVMs

## E-mail

* `Sender e-mail` - address from which should e-mail be sent 
* `SMTP server` - SMTP server address 
* `SMTP port` - SMTP server port 
* `SMTP SSL port` - SMTP SSL port \(if enabled\) 
* `SMTP user` - SMTP user used to send e-mails 
* `E-mail recipients (comma-separated)` - list of recipients of daily backup report
* `Daily backup report (sending time)` - time when daily backup report should be sent
* `Notify about recently failed backups` - if enabled, an e-mail will be sent if there were any failed export/store tasks within last 15 minutes

## License

This section enables you to view current license status and upload a new license if necessary. License details:

* `MAX_VMS_WITH_BACKUP` - maximum number of VMs that have at least one backup - when limit is reached you need to remove one of your existing VMs from repository if you want to backup another VM \(next rescan should add VM again to the repository\)
* `MAX_xxx_HOSTS` - maximum number of hosts for given platform
* `BP_xxx` - maximum number of backup destinations per backup provider type
* `EXPIRE_DATE` - trial period expiration date

## Authentication

This section enables you to setup LDAP authentication for administrative accounts in vProtect:

* `Enable LDAP authentication` - enables LDAP authentication, vProtect will try to check for non-existing accounts in LDAP, and if authenticated successfully, new account will appear on the list
* `Server URL` - LDAP server URL
* `Search` - Base DN that needs to be searched
* `User filter` - filter to be used to authenticate only users in a specific group
* `Use SSL` - enables SSL for LDAP connection
* `Use TLS` - enables TLS for LDAP connection
* `Anonymous access allowed` - if users are not allowed to anonymously browse LDAP directory you need to provide account which has that privilege:
  * `Service account DN` - DN of the user
  * `Service account password` - password of that user 

