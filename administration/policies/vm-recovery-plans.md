# Recovery Plans

Go to Policies from left menu and then go to Recovery Plan tab.

![](../../.gitbook/assets/restore-scheduled-recovery-plans-policy%20%281%29.jpg)

After clicking on ![](../../.gitbook/assets/icon-createpolicy_mini%20%285%29.jpg) provide name of policy and set priority. After saving you will be able to add rules using the new button on the right.

![](../../.gitbook/assets/restore-scheduled-recovery-plans-policy-save%20%281%29.jpg)

Click on it to create rule and customize restore settings.

![](../../.gitbook/assets/restore-scheduled-recovery-plans-policy-rules%20%281%29.jpg)

Each rule requires **name** for easier identification later

![](../../.gitbook/assets/restore-scheduled-recovery-plans-policy-rules-name%20%281%29.jpg)

In the **Virtual Environments** tab you need to select **Hypervisor type** for this rule and corresponding **virtual environments** of this type

![](../../.gitbook/assets/restore-scheduled-recovery-plans-policy-rules-virtual-environment%20%281%29.jpg)

If you previously defined any schedules for recovery plans you can select them in **Schedules** tab

![](../../.gitbook/assets/restore-scheduled-recovery-plans-policy-rules-schedule%20%281%29.jpg)

In **Restore Parameters** tab you specify where VMs are going to be restored - compared to regular restore parameters provided in manual restore window, notice that:

* you need to choose which backup to restore **-** last \(regardless of status\) or last successful
* you may want to use **Delete if Virtual Environment already exists** - which allows vProtect to remove VM with the same name as the one being restored

![](../../.gitbook/assets/restore-scheduled-recovery-plans-policy-rules-restore-parameters%20%281%29.jpg)

### You can also perform the same action thanks to the CLI interface: [CLI Reference](../cli-reference.md#recovery-plans-policies)

