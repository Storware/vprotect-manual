# Backup SLAs

To backup your application periodically:

![](../../.gitbook/assets/applications-backup-slas-policies.jpg)

* You need to create a Policy:
  * Go to Applications from the left side menu and then to Backup SLAs
  * Provide a policy name
  * Select your application from the list
  * Specify backup rule details - especially backup destination and schedules

![](../../.gitbook/assets/applications-backup-slas-policies-create.jpg)

* The second part is a schedule for policy:
  * Change tab from Policies to Schedules
  * Create a new schedule for application policy - create it just like other schedules, enter name, choose execution time \(time or interval\) and days of the week. \*optionally you can select policy if already exists

![](../../.gitbook/assets/applications-backup-slas-schedules.jpg)

![](../../.gitbook/assets/applications-backup-slas-schedules-create.jpg)

Now your application backups will be done periodically according to your policy.

