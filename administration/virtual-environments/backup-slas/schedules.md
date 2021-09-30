# Schedules

Schedules allow you to invoke specific policies periodically. This allows you to back up multiple VMs automatically.

A schedule defines when and on which days VMs should be backed up. To define a new schedule, open Backup SLAs under the Virtual Environments section and go to the Schedules tab, then use the ![](../../../.gitbook/assets/create-schedule%20%282%29.jpg) button.

![](../../../.gitbook/assets/backup-sla-schedules.jpg)

Now enter the properties:

![](../../../.gitbook/assets/backup-sla-schedules-create.jpg)

* `Schedule Active` - enable or disable executing schedule
* `Name` - schedule name
* `Backup Type` - defines the backup type: full or incremental
* `Execution Type` - choose the time or interval mode
* `Start Window` - defines for how long since the task start time scheduled tasks are allowed to be executed
* `Choose time of day` - for the time execution mode, this defines when the task should be added to the queue
* `Choose time of interval start` - for the interval execution mode, this defines when tasks should start
* `Choose time of interval end` - for the interval execution mode, this defines when tasks should end
* `Frequency` - defines how often the task will be executed during the interval
* `Choose days` - the last required parameter, select the days of the week on which the task will be performed

You can also use optional parameters to further personalize the backup time or select a virtual environment policy if it has been previously created.

## You can also perform the same action using the CLI interface: [CLI Reference](../../cli-reference.md#schedules)

