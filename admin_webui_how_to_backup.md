# Backup

## Manual Backup

Open "VIRTUAL MACHINES" tab, and click on ![](images/admin_webui_how_backup_icon_backup.png) to backup one VM.
![](images/admin_webui_how_backup_select_one.png)

Or select multiple VM's, and click on ![](images/admin_webui_how_backup_icon_backup_selected.png) to backup multiple VM's.
![](images/admin_webui_how_backup_select_multi.png)

Next choose backup type, destination, start time, priority, and click ![](images/admin_webui_how_backup_icon_blue_backup.png).
![](images/admin_webui_how_backup_chose_type_destination.png)

On admin task console you can see backup progress.
![](images/admin_webui_how_backup_task_console.png)


## Scheduled Backup

Open "VIRTUAL MACHINES" tab, and go to ![](images/admin_webui_how_backup_icon_virtual_machines_groups.png).
Next create new virtual machines group, click ![](images/admin_webui_how_backup_icon_create_group.png).
![](images/admin_webui_how_backup_virtual_machines_groups.png)


![](images/admin_webui_how_backup_vm_group.png)

* `Name` - virtual machines group name
* `Auto remove non-present VMs` - On - virtual machines added to virtual machines group, will be removed after delete it from vProtect.
* `Auto Assign Mode`
		`Disabled` - vProtect don't assign VM's to group.
		`Assign only` - vProtect automaticaly assign VM's to group.
		`Assign and remove` - vProtect automaticaly assign/remove VM's to group.
* `Create Include Rule` - vProtect automaticaly assign VM's.
* `Create Exclude Rule` - vProtect automaticaly remove VM's.
		`TAG based rule` - tag created, and assigned to VM on hypervisor.
		`Regex based rule` - common part of the name.
* `Select Backup Destination` - select one of previously created backup destination.
* `Choose VMs` - list of available VM's, what can be manually added to VM group.
* `Choose schedules` - list of created schedules, they can be assign to VM group.
* `Priority` - Priority of the task, 100 - high priority, 1 - low priority.

At the end save settings ![](images/admin_webui_save.png).

If you don't have created schedule, go to [schedules](admin_webui_schedules.md), and create new schedule.


