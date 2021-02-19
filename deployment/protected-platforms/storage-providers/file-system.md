# File system

vProtect is able to protect file systems mounted on its nodes. Both full and incremental backups are supported.

**Note:**

* vProtect scans for file system changes based on modification time and size of the file with each incremental backups.
* only regular files, symlinks and directories are backed up
* currently, for file-systems vProtect builds images for each backup before uploading data to the specific backup provider so that it doesn't have to upload objects one-by-one - these images will be merged automatically in restore tasks

Please complete the following steps to add a file system storage provider.

1. Go to `Storage` -&gt; `Infrastructure` and add click `Add Storage Provider`
2. Choose `File system` as a type and select node responsible for backup operations
3. Click `Save` - now you can define file systems in the `Storage` -&gt; `Instances` view

