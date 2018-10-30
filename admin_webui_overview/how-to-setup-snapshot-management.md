# How to setup snapshot management

vProtect can periodically create snapshots and keep several of them on every VM. In order to enable snapshot management for VM you need to do the following steps:

1. Go to Policies and create a new Snapshot Management policy
   * provide a **name** of the policy
   * optionally configure **auto-assignment** criteria so that your VMs can be automatically assigned by names or tags to this policy
   * assign **Virtual Machines** to your policy \(only VMs with snapshot management policy allow you to create snapshots manually or automatically from vProtect UI\)
   * specify **Rule** details - you need to provide retention settings - how many snapshots \(created by this policy\) will be kept and for how long
   * optionally choose snapshot management schedules if you created some already
2. Go to Schedules and define a new schedule:
   * apart from regular schedule settings - for snapshot management purposes it may be useful to use Interval based scheduling - to define schedules such as every 2h between 1 p.m. and 5 p.m.
   * optionally assign schedule to your policy - in most cases you want to be done automatically

Now snapshots will be done automatically according to the schedule. They also can be created manually if you go to your VM details and click button in the right corner \(the one with photo camera\). 

