# Virtual Environment Backup

Open "POLICIES" tab and click on ![](../../.gitbook/assets/icon-createpolicy_mini%20%283%29.jpg) button on the right.

![](../../.gitbook/assets/scheduled-backup-policies%20%281%29.jpg)

Now you should see the policy wizard with 5 main sections.

![](../../.gitbook/assets/scheduled-backup-policies-create%20%281%29.jpg)

## General

Under this section you can set up:

* Name of policy
* Switch on/off auto remove non-present virtual environments
* Set priority for tasks

## Auto-assignment

In this section you can configure automatic policy assignment based on certain criteria:

* Mode
  * Disabled
  * Assign only
  * Assign and remove
* Include or exclude rules based on hypervisor tag's or regular expression matching VM name.
* You can also select clusters to match only VMs that belong to them

## Virtual Environments

In this place, you can select virtual machines manually in a simple way

## Rule

This section is used to select the backup destination. If you have already created a schedule, you can also select it.

## Other

It is an optional section with two switches:

* Fail rest of the backup tasks if more than xx% of EXPORT tasks already failed
* Fail rest of the backup tasks if more than xx% of STORE tasks already failed

Two examples when using switches is useful It is very likely that if 30% of the backup tasks fail, the remaining tasks will also fail because the environment has failed. Or you are backing up a set of machines, and if even one is not secured, there is no point in backing up the rest.

At the end save settings.

### You can also perform the same action thanks to the CLI interface: [CLI Reference](../cli-reference.md#vm-backup-policies)

