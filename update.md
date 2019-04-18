# Update

**Note** that this process only applies for updates from version 3.x. Version 2.x requires a fresh 3.1 installation and database migration.

**Notice** update from version 3.6 to 3.7 require restart vprotect-server service after update.

`systemctl restart vprotect-server`



1. Download new RPM packages `vprotect-server-XXX.rpm` and `vprotect-node-XXX.rpm`
2. Copy Server RPM to host with vProtect Server installed
3. Update Server \(it may take a while, service is being restarted\):

   ```text
   yum -y update vprotect-server-XXX.rpm
   ```

4. If the node service was not running before update you may need to execute also:

   ```text
   systemctl start vprotect-server
   ```

5. Copy Node RPM to all hosts with vProtect Node installed
6. Update each Node:

   ```text
   systemctl stop vprotect-node
   yum -y update vprotect-node-XXX.rpm
   ```

7. If the node service was not running before update you may need to execute also:

   ```text
   systemctl start vprotect-node
   ```

8. Log into the web UI and check if nodes are running.
   * **Note** you may need to refresh your browser cache after update - for Chrome use `CTRL+SHIFT+R` \(Windows/Linux\) / `CMD+SHIFT+R` \(MacOS\)

