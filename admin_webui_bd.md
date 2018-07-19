# Backup destination management

Backup destination is a description of the backup provider configuration and retention settings. Each VM group can be backed up to the selected backup destination. Backup destinations must be assigned to the nodes in the node configuration.

**Note:** removal of any backup destination leaves data in the backup provider without option to re-attach it in the future.


## Available properties

vProtect handles retention for all backup destinations except NetBackup. There are 4 properties that define how long backup should be kept in backup destination:

* `Retention (Full) - no. of versions to keep` - number of full backups
* `Retention (Inc.) - no. of versions to keep` - number of incremental backups
* `Retention (Full) - no. of days to keep` - number of days to keep full backup
* `Retention (Inc.) - no. of days to keep` - number of days to keep incremental backup

Whichever condition is met first (either number of versions has been reached or backup is older than given limit), it is removed from backup destination.

Additionally, there are also pre/post access command execution settings - details: [How to setup pre/post BD access command execution](admin_webui_pre_post_bd_access_cmd.md)      

Each backup destination needs also a unique `Name` for easier identification in the configuration.


### File system

* `Storage paths` - list of paths accessible from the node where backups should be stored 
  * **note**: if more than one path is given vProtect will automatically balance each path and store file always in the path with the most free space available

### IBM Spectrum Protect

* `dsm.opt file path` - path to the `dsm.opt` configuration file on the node (ISP API must be installed on all nodes that use ISP backup destination
* `ISP node name` - name of the node in ISP server to be used
* `ISP node password` - password for the ISP node to be used
* `ISP Server time zone` - time zone used by ISP server
* `Progress refresh rate (1/x of data size) - backup` - how often should progress be updated during backup - `x` times per backup size
* `Progress refresh rate (1/x of data size) - restore` - how often should progress be updated during restore - `x` times per backup size


**Note** that all backup destinations need to use all the same settings - only retention may differ for each backup destination

### Veritas NetBackup

* `Client home path` - directory of the NetBackup client (needs to be installed and configured before running first tasks
* `Real export path`- NetBackup client requires export path (staging space, default /vprotect_data) to be real directories instead of symlinks; if `/vprotect_data` is a symlink to e.g. `/data` then put `/data` in this field
* `Policy name` - name of the policy assigned in NetBackup to the client used by vProtect
* `Schedule name` - name of the schedule assigned in NetBackup to the client used by vProtect; backup schedules are controlled by vProtect, so no specific values should be configured in NetBackup
* `Client name` - optional field when multiple nodes need to access same NetBackup Server - each node is supposed to backup to seprate backup destination (each pointing to the same NetBackup Server, but which different client name)

### Dell-EMC NetWorker

* `Server address` – address of your Networker server

### S3

* `API URL (optional)` - optional if Amazon S3 is used, otherwise provide API URL for your S3-compatible backup provider
* `Access key` - your S3 access key
* `Secret key` - your S3 secret key
* `Backup mode` - mode that specifies if backups should be stored in one bucket or in separate buckets for each VM
* `Bucket name (for SINGLE_BUCKET only)` - single bucket name to be used for backups
* `Bucket name prefix (for ONE_BUCKET_PER_VM only)` - prefix for bucket names used in amazon (must be globally unique), buckets will have names starting with this prefix - one for each VM 

### Microsoft Azure* `Account name` - account name used to access container
* `Account key` - account key used to access container

### OpenStack Swift* `Authentication URL` - v2 URL pointing to authentication service
* `User name` - username:tenant formatted username used by vProtect to log into OpenStack Swift
* `Password` - password used by vProtect to log into OpenStack Swift
* `Authentication method` - BASIC / TEMPAUTH / KEYSTONE
* `Segment no. length` - object name format to adress all segments (i.e. 5 implies maximum of 99999 segments for an object)
* `Segment size` - size of data segment into which data is being fragmented when sending to Swift
