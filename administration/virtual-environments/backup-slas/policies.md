# Policies

Policies allow you to group virtual machines in many ways. For example, based on the type of hypervisor.

To create a new backup policy, please open the Backup SLAs tab under the Virtual Environments section and click on ![](../../../.gitbook/assets/create-policy%20%281%29%20%281%29.jpg) button on the right.

![](../../../.gitbook/assets/backup-sla-policies.jpg)

Now you should see the policy wizard with 5 main sections.

![](../../../.gitbook/assets/backup-sla-policies-create.jpg)

## General

Under this section you can set up:

* The policy name
* Switch on/off auto-remove non-present virtual environments
* Set the priority for tasks

## Auto-assignment

In this section you can configure automatic policy assignment based on certain criteria:

* Mode
  * Disabled
  * Assign only
  * Assign and remove
* Include or exclude rules based on hypervisor tags or regular expressions matching the VM name, i.e.:
  * regular expression examples:
    * `.*` match any character any number of times
    * `vm-[0-9][0-9][0-9]` - match the name that starts with `vm-` and 3 digits
    * `(prod|uat|dev)-[0-9][0-9][0-9][a-z]?` - match the name that starts with `prod` or `uat` or `dev` prefix, then `-`, then 3 digits and an optional lower-case letter \(matching is case-sensitive\)
  * exclude rules always take precedence over include rules
  * VMs will not be reassigned to a different policy if they already have a matching policy assigned
  * VMs will be reassigned to a different policy only if the mode is `Assign and remove`, the current policy assignment rules don't match, and other's policy rules match
  * rules are joined with the OR operator, so 
    * if **any** rule \(tag or matched regular expression\) excludes the VM - it will be excluded
    * if **no** rule \(tag or matched regular expression\) excludes the VM, and **any** rule \(tag or matched regular expression\) includes the VM - it will be included
* You can also select clusters to match only VMs that belong to them.

## Virtual Environments

Here you can easily select virtual machines manually.

## Rule

This section is used to select the backup destination. If you have already created a schedule, you can also select it.

## Other

This is an optional section with two switches:

* Fail the rest of the backup tasks if more than xx% of the EXPORT tasks have already failed
* Fail the rest of the backup tasks if more than xx% of the STORE tasks have already failed

Two examples when using switches is useful It is very likely that if 30% of the backup tasks fail, the remaining tasks will also fail because the environment has failed. Or you are backing up a set of machines, and if even one is not secured, there is no point in backing up the rest.

At the end, save settings.

### You can also perform the same action using the CLI interface: [CLI Reference](../../cli-reference.md#vm-backup-policies)

