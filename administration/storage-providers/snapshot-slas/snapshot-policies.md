# Policies

In order to enable snapshot management for Storage Instance you need to do the following steps:

Go to Snapshot SLAs under Virtual Environment section and create a new Snapshot Management policy ![](../../../.gitbook/assets/create-policy%20%281%29%20%281%29%20%281%29.jpg)

![](../../../.gitbook/assets/snapshot-sla-policies.jpg)

As well as other types of Policies, you'll also find 4 main sections here.

![](../../../.gitbook/assets/snapshot-sla-policies-create.jpg)

## General

Under this section you can set up:

* Name of policy
* Switch on/off auto remove non-present virtual environments
* Set priority for tasks

## Auto-assignment

In this section you can set up:

* Mode
  * Disabled
  * Assign only
  * Assign and remove
* Include or exclude rules based on hypervisor tag's or regular expression matching Storage Instance name, i.e.:
  * regular expression examples:
    * `.*` match any character any number of times
    * `storage-[0-9][0-9][0-9]` - match name that starts with `storage-` and 3 digits
    * `(prod|uat|dev)-[0-9][0-9][0-9][a-z]?` - match name that starts with `prod` or `uat` or `dev` prefix, then `-`, then 3 digits and optional lower-case letter \(matching is case-sensitive\)
  * exclude rules always take precedence over include rules
  * Storage Instances may will not be reassigned to the different policy if they already have matching policy assigned
  * Storage Instances may will be reassigned to the different policy only if mode is `Assign and remove`, current policy assignment rules don't match, and other's policy rules match
  * rules are joined with OR operatorator, so 
    * if **any** rule \(tag or matched regular expression\) excludes Storage Instance - it will be excluded
    * if **no** rule \(tag or matched regular expression\) excludes Storage Instance, and **any** rule \(tag or matched regular expression\) includes Storage Instance - it will be included
* You can also select Storage Pools to match only Storage Instances that belong to them

## Storages

In this place, you can select storage instances manually in a simple way.

## Rule

Provide retention settings - how many snapshots \(created by this policy\) will be kept and for how long. If you have already created a schedule, you can also select it.

### You can also perform the same action thanks to the CLI interface: [CLI Reference](../../cli-reference.md#snapshot-management-policies)

