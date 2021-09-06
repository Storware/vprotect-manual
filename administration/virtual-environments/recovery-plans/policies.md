# Policies

To schedule a VM restore using a recovery plan \(or execute a recovery plan manually\), you must first create a policy.

Go to the Recovery Plans from the left menu under the Virtual Environments section.

![](../../../.gitbook/assets/virtual-environments-recovery-plans-policies.jpg)

After clicking on ![](../../../.gitbook/assets/create-policy%20%281%29%20%281%29%20%281%29.jpg) provide the name of the policy and set priority. After saving you will be able to add rules using the new button on the right.

![](../../../.gitbook/assets/virtual-environments-recovery-plans-policies-create.jpg)

Click on it and customize restore settings.

![](../../../.gitbook/assets/virtual-environments-recovery-plans-policies-create-rules.jpg)

Each rule requires a **name** for easier identification later

![](../../../.gitbook/assets/virtual-environments-recovery-plans-policies-create-rules2.jpg)

In the **Virtual Environments** tab, you need to select **Hypervisor type** for this rule and corresponding **virtual environments** of this type

![](../../../.gitbook/assets/virtual-environments-recovery-plans-policies-create-rules3.jpg)

If you previously defined any schedules for recovery plans you can select them in the **Schedules** tab

![](../../../.gitbook/assets/virtual-environments-recovery-plans-policies-create-rules4.jpg)

In **Restore Parameters** tab you specify where VMs are going to be restored - compared to regular restore parameters provided in the manual restore window, notice that:

* you need to choose **\*\*which backup to restore -** last \(regardless of status\) **or** last successful
* you may want to use **Delete if Virtual Environment already exists** - which allows vProtect to remove VM with the same name as the one being restored

![](../../../.gitbook/assets/virtual-environments-recovery-plans-policies-create-rules5.jpg)

