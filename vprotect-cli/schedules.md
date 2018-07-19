# Schedules

Schedule management module is used to create schedules associated with VMs or groups of VMs. Each schedule defines days of week and the hour when the backup tasks should be invoked. Each schedule also defines time window in which the backup process must start. If the task queue is full and the tasks starts after the specified amount of time from the schedule – it will be cancelled and marked as failed.

Notice:

* tasks in the queue are run in the order from the highest to the lowest priority.
* you can assign multiple schedules if you need backup to be executed at different hours on different days
* remember always to assign schedule to VM or VM group – schedule can be assigned only to a single VM or VM group

To manage schedules in the system use `vprotect sched` sub-command.

```text
[root@vProtect3 ~]# vprotect sched
Incorrect syntax: Missing required option: [-a Set schedule to be active (1) / not active (0), -c Create schedule for VM (type: FULL / INCREMENTAL), -d Delete schedule, -l List schedules, -m Modify schedule (type: FULL / INCREMENTAL)]

usage: sched -a <GUID> <0|1> | -c <NAME> <TYPE> <START_TIME> <DURATION> <everyday | DAYS_OF_WEEK> | -d <GUID> | -l | -m <GUID> <NAME> <TYPE> <START_TIME> <DURATION>
       <everyday | DAYS_OF_WEEK>
Schedule management
 -a,--set-active <GUID> <0|1>                                                         Set schedule to be active (1) / not active (0)
 -c,--create <NAME> <TYPE> <START_TIME> <DURATION> <everyday | DAYS_OF_WEEK>          Create schedule for VM (type: FULL / INCREMENTAL)
 -d,--delete <GUID>                                                                   Delete schedule
 -l,--list                                                                            List schedules
 -m,--modify <GUID> <NAME> <TYPE> <START_TIME> <DURATION> <everyday | DAYS_OF_WEEK>   Modify schedule (type: FULL / INCREMENTAL)
```

## Examples

* To list all schedules

  ```text
  vprotect shed -l
  ```

* To create a full backup schedule with name Schedule1 executed every day at 05:00 with 60 minutes of time window:

  ```text
  vprotect sched –c Schedule1 FULL 05:00 60 everyday
  ```

* To create a incremental backup schedule with name Schedule2 executed every Monday, Wednesday and Friday at 17:00 with 90 minutes of time window:

  ```text
  vprotect sched -c Schedule2 INCREMENTAL 17:00 90 mon,wed,fri
  ```

  **Note** that days of week are given as a single comma-separated string of short \(3-letter\) weekday names

* To create schedules \(full backup\) with name Schedule3 and Schedule4 executed every Tuesday at 02:00 and every Wednesday at 03:00 with 90 minutes of time window and assign them to the VM group with ID 123:

  ```text
  vprotect sched –c Schedule3 FULL 02:00 90 tue
  vprotect sched –c Schedule4 FULL 03:00 90 wed
  vprotect vmg -s 3afcd507-a4f5-484d-8d34-53c73d7a5809 8d8649cf-d3a1-4e54-b58b-69e51b2456e2,e694dba7-0fdf-44dc-beec-3c9e480b0621
  ```

  assuming GUIDs generated for new schedules `8d8649cf-d3a1-4e54-b58b-69e51b2456e2` and `e694dba7-0fdf-44dc-beec-3c9e480b0621` respectively:

* To disable a schedule with GUID `6651787d-9a55-421d-8158-ead80a70a9cb`:

  ```text
  vprotect sched -a 6651787d-9a55-421d-8158-ead80a70a9cb 0
  ```

