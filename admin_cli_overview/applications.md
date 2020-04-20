# Applications

Application management module is used to provide information about apps that has been defined report status of last backup of your apps \(and all backups for a particular app\).

To manage Applications in the system use `vprotect app` sub-command.

```text
[root@vprotect ~]# vprotect app
Incorrect syntax: Missing required option: [-sC Set SSH access credentials, -A Assign application backup policy, -c Create a new application, -d Delete an application, -sH Set SSH access host/port, -g Show application details, -xC Set application CMD exec configuration, -sN Set application node, -l List applications, -L List backups of the application, -m Modify application]

usage: app -A <GUID> <APP_POLICY_GUID> | -c <NAME> <CONFIG_GUID> | -d <GUID> | -g <GUID> | -l | -L <GUID> | -m <GUID> <NAME> | -sC <GUID> <SSH_USER> <SSH_PASS> | -sH <GUID> <SSH_HOST> <SSH_PORT> | -sN <GUID>
       <NODE_GUID> | -xC <GUID> <APP_CMD_EXEC_CONFIG_GUID>
Application backup management
 -A,--assign-app-policy <GUID> <APP_POLICY_GUID>                   Assign application backup policy
 -c,--create <NAME> <CONFIG_GUID>                                  Create a new application
 -d,--delete <GUID>                                                Delete an application
 -g,--details <GUID>                                               Show application details
 -l,--list                                                         List applications
 -L,--list-backups <GUID>                                          List backups of the application
 -m,--modify <GUID> <NAME>                                         Modify application
 -sC,--set-ssh-credentials <GUID> <SSH_USER> <SSH_PASS>            Set SSH access credentials
 -sH,--set-ssh-host <GUID> <SSH_HOST> <SSH_PORT>                   Set SSH access host/port
 -sN,--set-node <GUID> <NODE_GUID>                                 Set application node
 -xC,--set-app-cmd-exec-config <GUID> <APP_CMD_EXEC_CONFIG_GUID>   Set application CMD exec configuration
```

## Examples

* To list all Applications

  ```text
  vprotect app -l
  ```

* To show details of the given Application \(by GUID\)

  ```text
  vprotect app -g 0f36f40c-6427-4035-9f2b-1ead6aca3597
  ```

* To add Application \(first GUID\) to the given policy \(second GUID\):

  ```text
  vprotect app -A 0f36f40c-6427-4035-9f2b-1ead6aca3597 3afcd507-a4f5-484d-8d34-53c73d7a5809
  ```

* To show backup history of a VM with given GUID:

  ```text
  vprotect app -L 0f36f40c-6427-4035-9f2b-1ead6aca3597
  ```

