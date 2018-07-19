# Task management

Task management module is used to show and cancel tasks being executed. You can also monitor progress of the tasks.**Notice**:
* tasks in the queue are run in the order from the highest to the lowest priority.* queue is being periodically cleaned, so only last tasks are being shown
To manage tasks use `vprotect task` sub-command. 

```
[root@localhost vprotect]# vprotect task
Incorrect syntax: Missing required option: [-Q List queued tasks, -R List running tasks, -d Delete/cancel task, -F List finished (including failed) tasks, -g Show task details, -l List tasks (this node only), -L List tasks (all nodes and not assigned to any node)]

usage: task -d <GUID> | -F | -g <GUID> | -l | -L | -Q | -R
Task management
 -d,--delete <GUID>    Delete/cancel task
 -F,--list-finished    List finished (including failed) tasks
 -g,--details <GUID>   Show task details
 -l,--list             List tasks (this node only)
 -L,--list-all         List tasks (all nodes and not assigned to any node)
 -Q,--list-queued      List queued tasks
 -R,--list-running     List running tasks
```	

## Examples
* To list tasks assigned to the current node:

  ```
vprotect task -l
  ```
	
* To list all tasks in the system:

  ```
vprotect task -L
  ```

* To show task details:

  ```
vprotect task -g 091d623d-edea-490f-8b3b-da30c09a3f93
  ```  
* •	To cancel the task with ID `091d623d-edea-490f-8b3b-da30c09a3f93`: 
  ```
vprotect task -d 091d623d-edea-490f-8b3b-da30c09a3f93
  ```**Notice** – some tasks may require to be finished before they are cancelled, i.e. export VM from the hypervisor – after cancellation it may take some time for task to process cancel request and clean up snapshots etc. if necessary - backup will be marked as failed.
