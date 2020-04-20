# How to backup

## Manual Backup

Open "VIRTUAL MACHINES" tab, and click on ![](../.gitbook/assets/admin_webui_how_backup_icon_backup.png) to backup one VM.

![](../.gitbook/assets/admin_webui_how_backup_select_one.png)

Or select multiple VM's, and click on ![](../.gitbook/assets/admin_webui_how_backup_icon_backup_selected%20%281%29.png) to backup multiple VM's.

![](../.gitbook/assets/admin_webui_how_backup_select_multi.png)

Next choose backup type, destination, start time, priority, and click ![](../.gitbook/assets/admin_webui_how_backup_icon_blue_backup%20%281%29.png).

![](../.gitbook/assets/admin_webui_how_backup_chose_type_destination%20%281%29.png)

On admin task console you can see backup progress.

![](../.gitbook/assets/admin_webui_how_backup_task_console%20%281%29.png)

## Scheduled Backup

Open "Policies" tab, and go to ![](https://github.com/Storware/vprotect-manual/tree/cfa9bb2cf117ac097f14edc5da966ec14b20b951/images/admin_webui_how_backup_icon_virtual_machines_groups.png). Next create new Policy, click ![](../.gitbook/assets/create_policy.png) .

![](../.gitbook/assets/admin_webui_how_backup_virtual_machines_groups%20%281%29.png)

![](../.gitbook/assets/admin_webui_how_backup_vm_group%20%281%29.png)

* `Name` - virtual machines group name
* `Auto remove non-present VMs` - On - virtual machines added to virtual machines group, will be removed after delete it from vProtect.
* `Auto Assign Mode`

  ```text
    `Disabled` - vProtect don't assign VM's to group.
    `Assign only` - vProtect automaticaly assign VM's to group.
    `Assign and remove` - vProtect automaticaly assign/remove VM's to group.
  ```

* `Create Include Rule` - vProtect automaticaly assign VM's.
* `Create Exclude Rule` - vProtect automaticaly remove VM's.

  ```text
    `TAG based rule` - tag created, and assigned to VM on hypervisor.
    `Regex based rule` - common part of the name.
  ```

* `Select Backup Destination` - select one of previously created backup destination.
* `Choose VMs` - list of available VM's, what can be manually added to VM group.
* `Choose schedules` - list of created schedules, they can be assign to VM group.
* `Priority` - Priority of the task, 100 - high priority, 1 - low priority.

At the end save settings ![](../.gitbook/assets/admin_webui_save.png).

If you don't have created schedule, go to [schedules](admin_webui_schedules.md), and create new schedule.

