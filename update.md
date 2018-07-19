# vProtect 3.x update

**Note** that this process only applies for updates from version 3.x. Version 2.x requires a fresh 3.1 installation and database migration.

1. Download new RPM packages `vprotect-server-XXX.rpm` and `vprotect-node-XXX.rpm`
1. Copy Server RPM to host with vProtect Server installed
1. Update Server (it may take a while, service is being restarted):

  ```
yum -y update vprotect-server-XXX.rpm
  ```

1. If the node service was not running before update you may need to execute also:

  ```
systemctl start vprotect-server
  ```

1. Copy Node RPM to all hosts with vProtect Node installed
1. Update each Node:

  ```
yum -y update vprotect-node-XXX.rpm
  ```

1. If the node service was not running before update you may need to execute also:

  ```
systemctl start vprotect-node
  ```

1. Log into the web UI and check if nodes are running.
  * **Note** you may need to refresh your browser cache after update - for Chrome use `CTRL+SHIFT+R` (Windows/Linux) / `CMD+SHIFT+R` (MacOS)