# Node configuration management

Use `vprotect config` sub-command add/remove backup destinations from node config, list or show details of node configs. 

**Note**: configuration changes, as well as assigning configuration to the nodes can be done _from web UI only_.

```
[root@localhost vprotect]# vprotect config
Incorrect syntax: Missing required option: [-b Add backup destinations, -r Remove backup destinations, -g Show config details, -l List node configs]

usage: config -b <CONFIG_GUID> <[BD_GUID|BD_NAME],...,[BD_GUID|BD_NAME]> | -g <GUID> | -l | -r <CONFIG_GUID> <[BD_GUID|BD_NAME],...,[BD_GUID|BD_NAME]>
Node configuration management
 -b,--add-backup-destinations <CONFIG_GUID> <[BD_GUID|BD_NAME],...,[BD_GUID|BD_NAME]>      Add backup destinations
 -g,--details <GUID>                                                                       Show config details
 -l,--list                                                                                 List node configs
 -r,--remove-backup-destinations <CONFIG_GUID> <[BD_GUID|BD_NAME],...,[BD_GUID|BD_NAME]>   Remove backup destinations
```	

## Examples
* To list all node configurations

  ```
vprotect config -l
  ```
	
* To show details of the given node config (by GUID)

  ```
vprotect config -g 40212820-880e-11e7-bddf-000c2912fc7b
  ```
	
* To add backup destination (second GUID) to the given node configuration (first GUID):

  ```
vprotect config -b 40212820-880e-11e7-bddf-000c2912fc7b f27b6018-ce1c-4d67-991e-31d9f4f6662b
  ```
	
	More backup destinations can be passwd by separating GUIDs with commas.
	
	