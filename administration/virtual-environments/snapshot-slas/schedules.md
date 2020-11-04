# Schedules

Schedule defines when and on which days snapshots should be created. To define new schedule use the ![](../../../.gitbook/assets/create-schedule.jpg)

![](../../../.gitbook/assets/snapshot-sla-schedules.jpg)

Now provide properties:

![](../../../.gitbook/assets/snapshot-sla-schedules-create.jpg)

* `Schedule Active` - enable or disable executing schedule
* `Name` - schedule name
* `Execution Type` - choose time or interval mode
* `Start Window` - defines for how long since task start time scheduled tasks are allowed to be executed
* `Choose time of day` - for time execution mode defines when the task should be added to the queue
* `Choose time of interval start` - for interval execution mode defines when tasks should start
* `Choose time of interval end` - for interval execution mode defines when tasks should end
* `Frequency` - defines how often the task will be executed during the interval
* `Choose days` - last required parameter, select days of the week on which the task will be performed

You can also use optional parameters to further personalize the backup time or select a virtual environment policy if it has been previously created.

