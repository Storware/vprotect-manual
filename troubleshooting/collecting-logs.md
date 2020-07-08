# Collecting logs

## Using Web UI

### General logs

1. Go to the `Logs` section \(top bar\)
2. You'll see directories for `api`, `appserver` and one directory for each node \(same as their names\)
3. To download all logs - click button `Download all logs` which will generate archive containing all logs\)
4. To download specific file - browse to the directory and click name of the log

### Backup-related logs

1. Go to the VM details and `backup history` tab
2. Click second icon on the right of the backup you want to collect logs for
3. Downloaded file is `vprotect_daemon.log` file filtered, so it should contain only entries related to the tasks of backup/restore operations related to this backup 

## Directly from the operating system

### vProtect Node

Log files are stored in this folder: `/opt/vprotect/logs/<node_name>`:

* `vprotect_client.log` - stores CLI-related messages
* `vprotect_daemon.log` - stores vProtect Node's engine-related message

### vProtect Server

Log files are stored in:

* `/opt/vprotect/logs/appserver` - application server \(hosting vProtect Server\) messages
* `/opt/vprotect/logs/api` - vProtect Server application logs 

### Hyper-v Agent

For Hyper-v agent, logs are stored in this folder: `c:\Program Files\HyperVAgent\Hyper-v Agent\bin\Logs`

