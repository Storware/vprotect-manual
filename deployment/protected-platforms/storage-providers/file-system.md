# File system

## General

vProtect is able to protect file systems mounted on its nodes. Both full and incremental backups are supported.

**Note:**

* vProtect scans for file system changes based on the modification time and size of the file with each incremental backup.
* only regular files, symlinks and directories are backed up
* for file-systems, vProtect currently builds images for each backup before uploading data to the specific backup provider, so it doesn't have to upload objects one-by-one - these images will be merged automatically in restore tasks

Please complete the following steps to add the file system storage provider.

1. Go to `Storage` -&gt; `Infrastructure` and click `Add Storage Provider`
2. Choose `File system` as a type and select the node responsible for backup operations
3. Click `Save` - now you can define file systems in the `Storage` -&gt; `Instances` view

