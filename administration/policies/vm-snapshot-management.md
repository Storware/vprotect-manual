# VM Snapshot Management

Go to Policies and create a new Snapshot Management policy ![](../../.gitbook/assets/icon-createpolicy_mini%20%284%29.jpg)

![](../../.gitbook/assets/snapshot-management%20%281%29.jpg)

As well as other types of Policies, you'll also find 4 main sections here.

![](../../.gitbook/assets/snapshot-management-create-policies%20%281%29.jpg)

## General

Under this section you can set up:

* Name of policy
* Switch on/off auto remove non-present virtual environments
* Set priority for tasks

## Auto-assignment

In this section you can set up:

* Policy's work mode

  * Disabled
  * Assign only
  * Assign and remove

  _If you decide to second or third work mode, you can create include or exclude rules based on hypervisor tag's or regex rule based on virtual machine name._

* You can also select a cluster to select all virtual machines that belong to it

## Virtual Environments

In this place, you can select virtual machines manually in a simple way.

## Rule

Specify **Rule** details - you need to provide retention settings - how many snapshots \(created by this policy\) will be kept and for how long. If you have already created a schedule, you can also select it.

### You can also perform the same action thanks to the CLI interface: [CLI Reference](../cli-reference.md#snapshot-management-policies)

