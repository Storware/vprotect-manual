# How to use recovery plans

Recovery plans are used to automate DR process, so that vProtect executes multiple restore operations to the target environment with preconfigured settings.

Recovery plans can be executed on-demand or on a scheduled basis \(for instance to test recovery process periodically\). Recovery plans consist of rules, each one for a particular virtualization platform, which specify VMs, restore settings and optionally schedules. Only rules marked as active are executed.

Recovery plan rule specifies which backup is going to be restored:

* last \(regardless of the status\) - mostly for periodic tests - will try to use last backup, even if it was failed \(which will result in a failed restore task as well\) to indicated status of DR for periodic tests
* last successful - for actual DR, takes last successful backup

## Defining recovery plan

1. Open **Policies** from left menu
2. Go to the **Recovery Plan** tab
3. Click **Create** button
4. Provide name of the recovery plan and click Save
5. Now you'll be able to add rules with button on the right:
   * Each rule requires **name** for easier identification later
   * rule marked as **active** by default
   * in the **Virtual Environments** tab you need to select **Hypervisor type** for this rule and corresponding **virtual environments** of this type
   * if you previously defined any schedules for recovery plans you can select them in **Schedules** tab
   * in **Restore Parameters** tab you specify where VMs are going to be restored - compared to regular restore parameters provided in manual restore window, notice that:
     * you need to choose ****which backup to restore - **last \(regardless of status\)** or **last successful**
     * you may want to use **Delete if Virtual Environment already exists** - which allows vProtect to remove VM with the same name as the one being restored
6. Now you can execute recovery plan on demand from the **Recovery Plan** tab in **Policies** section - this should generate restore tasks according to your settings, 
   * if you want to invoke recovery plan according to your schedules - create them in **Schedules** section \(type must be **Recovery Plan**\) and assign the **rules** in your recovery plans

