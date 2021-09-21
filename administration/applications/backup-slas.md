# Backup SLAs

To back up your application periodically:

![](../../.gitbook/assets/applications-backup-slas-policies.jpg)

* You need to create a Policy:
  * Go to Applications on the left side menu and then to Backup SLAs
  * Provide a policy name
  * Select your application from the list
  * Specify the backup rule details - especially the backup destination and schedules

![](../../.gitbook/assets/applications-backup-slas-policies-create.jpg)

* The second part is a schedule for the policy:
  * Change from the Policies tab to the Schedules tab
  * Create a new schedule for the application policy - create it just like other schedules, that is enter the name, choose the execution time \(time or interval\) and the days of the week. \*Optionally, you can select a policy if it already exists.

![](../../.gitbook/assets/applications-backup-slas-schedules.jpg)

![](../../.gitbook/assets/applications-backup-slas-schedules-create.jpg)

Now your application backups will be done periodically according to your policy.

