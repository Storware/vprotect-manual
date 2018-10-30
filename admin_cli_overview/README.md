# vProtect CLI

## Overview

Every node provides CLI that can be used to manage configuration and invoke tasks on vProtect Node. All of the commands are executed using `vprotect` command. The general syntax is as shown below:

```text
[root@vProtect3 ~]# vprotect
usage: vprotect <COMMAND> -<ARG_1> ... -<ARG_N>

COMMAND is one of the following:
 node     		Node management
 config   		Node configuration management
 hv       		Hypervisor management
 hvm      		Hypervisor manager management
 hc       		Hypervisor cluster management
 hs       		Hypervisor storage management
 vm       		Virtual machine management
 vmpolicy 		VM backup policy management
 bd       		Backup destination management
 sched    		Schedule management
 brvm     		VM backup & restore
 brapp    		Application backup & restore
 mnt      		Mounted backup management
 task     		Task management
 login    		User login
 logout   		Node and user logout
 stop     		Safely stops node
 snappolicy		Snapshot policy management
 app      		Application backup management
 appconf  		App command execution management
 apppolicy		Application backup policy management
 status			Shows node status
 start			Starts node
```

## Starting and stopping node

* To check if vProtect Node is running, run:

  ```text
  systemctl status vprotect-node
  ```

  or

  ```text
  vprotect status
  ```

* To start vProtect Node run:

  ```text
  systemctl start vprotect-node
  ```

  or

  ```text
  vprotect start
  ```

* To safely stop vProtect Node \(this command waits for all tasks to be cancelled, so that temporary objects are cleaned up\) run:

  ```text
  systemctl stop vprotect-node
  ```

  or

  ```text
  vprotect stop
  ```

* In emergency cases you may need to kill the engine without task clean up - get PID of the node by running:

  ```text
  vprotect status
  ```

  and kill process using:

  ```text
  kill <PID>
  ```

## User login

* To log in to CLI use the following command:

  ```text
  vprotect login -u USER_NAME
  ```

where `USER_NAME` is your admin account in vProtect. You will be prompted for password. From this moment you are ale execute commands.

* Once you completed working with CLI, you can log out by running:

  ```text
  vprotect logout -u
  ```

## Note about GUIDs

All of the commands require GUID of the object that you want to refer to in command. GUID is found in the first column of typical list command, e.g.:

1. Let's list VMs and get VM GUID:

   ```text
    [root@vProtect3 ~]# vprotect vm -l
                    GUID                              Name                   Hypervisor       Present   VM group    Protected       Last backup        
    ------------------------------------  -----------------------------  -------------------  -------  -----------  ---------  ----------------------  
    0f36f40c-6427-4035-9f2b-1ead6aca3597  CentOS 7 (1)                   dXenServer72         true     PM-T1_Group  false      2017-10-03 00:30 (Tue)
   ```

2. Now let's show details of the VM:

   ```text
    [root@vProtect3 ~]# vprotect vm -g 0f36f40c-6427-4035-9f2b-1ead6aca3597
    Property                 Value                                                
    -----------------------  ---------------------------------------------------  
    GUID                     0f36f40c-6427-4035-9f2b-1ead6aca3597  
    Name                     CentOS 7 (1)  
    UUID                     5a2987f4-c785-25b1-f7e5-fa2396026d02  
    Present                  true  
    HV type                  CITRIX  
    HVM type                 -  
    Inc. backup snapshot ID  -  
    Tags                     []  
    Hypervisor               dXenServer72 (adfb98f3-ab65-42a0-95b2-78cad0f298b0)  
    HV manager               -  
    Backups                  111  
    VM group                 PM-T1_Group (982d2d3f-efb1-41ca-a651-2bd73e2eeb6e)  
    Protected                false  
    Last backup              2017-10-03 00:30 (Tue)
   ```

**Note**: that UUID is the ID used by hypervisor or HV manager, while GUID is the ID used by vProtect to uniquely identify objects.

Node GUID can be replaced by `this` keyword to state that action such as restore or mount is going to be run on node where the command is executed from. For instance, to restore backup with given GUID on current node to `/vprotect_data`:

```text
vprotect br -r 2132182d-e9ab-4478-a1db-48222b0e515b this /vprotect_data
```

Similarly, backup destinations can be referred by names. So backup of some VM with given GUID to backup destination with name `ISP` can be done like this:

```text
vprotect br -b b6f96d43-6f55-468f-a7a0-8aa6219fdf4e ISP
```

