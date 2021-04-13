# Backup SLAs

Backup SLAs allow you to group backup policies and schedules for multiple VMs. In general, VM can have at most only one backup policy to always have easy-to-interpret configuration.

Each policy can have multiple schedules assigned, so that you can define more complex schedules, so that backups are executed even multiple times each day and with different backup type.

**Note:**

* schedules define the type of a backup - full or incremental
* currently **incremental forever** backup approach is **not supported**, as it may result in a very long chain, which takes a considerable amount of time during restore procedures \(data merge\)
* we highly recommend the approach is to create a schedule for **periodic full** backup and always assign at least 1 such schedule in backup SLAs
* in order to create incremental backups, you need always to have **at least 1 incremental backup schedule** and run at least one full backup
* in most virtualization platforms supported by vProtect, it is required to **keep the last snapshot** for future incremental backups

