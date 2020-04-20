# Application backup policies

Application backup policies management module is used to define backup policies for Applications. You can assign different backup priority for a policy when the scheduler invokes backup task. You need first to define application backup policy and then add Applications to it. Application can belong only to a single backup policy.

To manage Application policies in the system use `vprotect apppolicy` sub-command.

**Note**: it is important to assign backup destination for a policy \(required for node to know where to store backups\)

Application backup policies are supposed to have multiple rules \(but currently vProtect supports exactly 1 rule\). Rule specifies schedules and backup destination for the policies.

```text
[root@localhost ~]# vprotect apppolicy
Incorrect syntax: Missing required option: [-p Set application backup priority, -rR Remove rules from policy, -A Assign applications to policy, -aR Add new rule to selected policy, -r List rules for application backup policy, -c Create application backup policy, -d Delete application backup policy, -U Unassign applications from policy, -g Show application backup policy details, -l List application backup policies, -m Modify application backup policy]

usage: apppolicy -A <GUID> <APP_GUID,...,APP_GUID> | -aR <NAME> <POLICY_GUID> <SCHEDULE_GUID> <BACKUP_DESTINATION_GUID> | -c <NAME> <PRIORITY> | -d <GUID> | -g <GUID> | -l | -m <GUID> <NAME> <PRIORITY> | -p <GUID>
       <PRIORITY> | -r <GUID> | -rR <POLICY_GUID> <RULE_GUID,...,RULE_GUID> | -U <GUID> <APP_GUID,...,APP_GUID>
Application backup policy management
 -A,--Assign applications <GUID> <APP_GUID,...,APP_GUID>                         Assign applications to policy
 -aR,--add-rule <NAME> <POLICY_GUID> <SCHEDULE_GUID> <BACKUP_DESTINATION_GUID>   Add new rule to selected policy
 -c,--create <NAME> <PRIORITY>                                                   Create application backup policy
 -d,--delete <GUID>                                                              Delete application backup policy
 -g,--details <GUID>                                                             Show application backup policy details
 -l,--list                                                                       List application backup policies
 -m,--modify <GUID> <NAME> <PRIORITY>                                            Modify application backup policy
 -p,--set-priority <GUID> <PRIORITY>                                             Set application backup priority
 -r,--list-rules <GUID>                                                          List rules for application backup policy
 -rR,--remove-rules <POLICY_GUID> <RULE_GUID,...,RULE_GUID>                      Remove rules from policy
 -U,--Unassign applications <GUID> <APP_GUID,...,APP_GUID>                       Unassign applications from policy
```

## Examples

* To list all application backup policies

  ```text
  vprotect apppolicy -l
  ```

* To show details of the given VM \(by GUID\)

  ```text
  vprotect apppolicy -g 3afcd507-a4f5-484d-8d34-53c73d7a5809
  ```

* To create a new policy

  ```text
  vprotect apppolicy -c "My Policy"
  ```

* To set new backup destination \(you can use name or GUID\) for a policy \(first GUID\) you need to remove old rule and assign a new one \(last command requires GUIDs of policy, schedule and new backup destination\):

  \`\`\`text \[root@localhost ~\]\# vprotect apppolicy -r e99f1379-ce84-4127-af98-81f35c457dff b48ca199-a1ad-43b0-b085-368d25d63bd6

  ```text
              GUID                   Name    No. of schedules  Backup destinations  
  ```

  b48ca199-a1ad-43b0-b085-368d25d63bd6 Default 1 -

\[root@localhost ~\]\# vprotect apppolicy -rR e99f1379-ce84-4127-af98-81f35c457dff b48ca199-a1ad-43b0-b085-368d25d63bd6

Property Value

GUID e99f1379-ce84-4127-af98-81f35c457dff  
Name vProtect DB Backup Policy  
Priority 50  
Applications \[\]  
Backup rules 0

\[root@localhost ~\]\# vprotect apppolicy -r e99f1379-ce84-4127-af98-81f35c457dff b48ca199-a1ad-43b0-b085-368d25d63bd6

\[root@localhost ~\]\# vprotect apppolicy -aR Default e99f1379-ce84-4127-af98-81f35c457dff f733ad53-0b4e-4152-bff8-c7508e5720f8 59160f5e-ba56-4db7-8489-2a5a632f7ba2

Property Value

GUID e99f1379-ce84-4127-af98-81f35c457dff  
Name vProtect DB Backup Policy  
Priority 50  
Applications \[\]  
Backup rules 1

\`\`\`

\`\`

