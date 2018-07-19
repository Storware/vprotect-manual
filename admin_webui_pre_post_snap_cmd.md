# Pre/post snapshot command execution on VM

1. Prepare your scripts
  * pre script is being invoked before snapshot is taken
  * post script is executed after snapshot is done (or if snapshot creation fails)

1. Upload your scripts to the VM

1. Add VM fingerprint to SSH known_hosts on the **node**:

  * it must be `known_hosts` file that belongs to `vprotect` user
  * algorithm must be set to `ssh-rsa`
  * `VM_USER` must have sufficient privileges to execute script on the `VIRTUAL_MACHINE`

  **Example:**
	
	```
sudo -u vprotect ssh -o HostKeyAlgorithms=ssh-rsa VM_USER@VIRTUAL_MACHINE
   ```
   
1. Open "VIRTUAL MACHINES" section from the left menu:
![](images/admin_webui_how_mount_select_one.png)

1. Click VM name to open VM details page

1. Scroll down to tabs and open ![](images/admin_webui_vm_details_settings.png) tab.

1. Enable pre/post command execution and provide command arguments (first argument is the command itself):
![](images/admin_webui_vm_details_settings_pre_post_snap_cmd.png) 

1. Provide SSH host/port/user information:
![](images/admin_webui_vm_details_settings_ssh_host_port_user.png)

1. Click ![](images/admin_webui_save.png) button

1. Provide SSH password and click `Change Password`:
![](images/admin_webui_vm_details_settings_ssh_password.png)

1. Initiate backup to test it the scripts have been executed correctly - in the `vprotect_daemon.log` files you should be able to see messages like this:

  ```
2018-05-04 14:31:26.303  INFO
[1207e9f3-dfb5-4d19-8851-b7cf4af57543] Executing pre/post snapshot command: [/root/post.sh arg1 "arg 2"]
  ```