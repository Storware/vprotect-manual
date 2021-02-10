# Integration

In this section, we're going to highlight key aspects necessary to integrate 3rd party solutions with vProtect. We assume a typical scenario, where you want to invoke vProtect operations on behalf of a user of a self-service portal. We assume that end-user uses the above-mentioned portal for a subset of administrative actions, and we want to give user ability to perform basic backup related operations.

These include:

* listing VMs in inventory \(including already non-existing\)
* getting VM details
* performing backup of a VM on the list
* browse backup history
* restore specified VM
* monitor progress of each operation
* manage policies and schedules

## Architecture

The architecture below shows key components, communication and data flows. All communication between 3rd party system goes via RESTful API exposed by vProtect Server. Tasks are being performed by the Node behind the scenes. End-user is going to use only a 3rd party system to invoke and monitor the status of the tasks.

Multi-tenancy and permission handling is on the 3rd-party system side. There is however `tenantID` field in several cases which can be used to assign objects to the tenant on the 3rd-party system side, later to be used in filter criteria.

3rd-party system must use a dedicated vProtect account to perform operations.

![](.gitbook/assets/vprotect-detailed-architecture%20%281%29%20%281%29.png)

## Integration steps

### Setup

Your system is going to communicate over HTTPS which by default runs on port 8181, but during the setup can optionally be exposed on 443 as well. You need to generate an SSL certificate as described [here](deployment/common-tasks/enabling-https-connectivity-for-remote-nodes.md).

vProtect can invoke operations only on VMs that exist in its inventory. It is being synced periodically so that it reflects changes in the virtualization platform.

REST API can either be invoked directly or using generated client for Java. Please contact us to receive the current client version matching your language.

Base URL for API calls is: `https://VPROTECT_SERVER:PORT/api`. We'll assume that all endpoints are prefixed with the base URL it in the rest of this guide. In this guide we'll focus on the integration process, and skip actual requests and responses here and - please check API docs for details.

Quite often in requests or responses, vProtect requires `NameAndGuid` to refer to other objects. `GUID` is the ID that you can use later to get additional information about i.e. hypervisor or backup. For convenience, we provide `name` to present it on the list views.

When you need to provide `NameAndGuid` in requests, you actually need to pass the object that has just `GUID` provided.

A similar concept applies to enum types - vProtect uses `EnumNameAndDescription` with `name` as enum name and `description` to show it to the end-user in a more user-friendly way. Anywhere when you are required to use enums such as type, state, days of week etc. you need to use `EnumNameAndDescription` object. In requests, you need to provide just `name`.

### Login

When you want to invoke APIs, you need to be authenticated first. vProtect API exposes an endpoint that you need to send login and password first to the `POST /session/login` endpoint. Save cookies so that you'll be able to invoke the next calls.

If you ever call any end-point without having a valid session, you'll receive 401 Unauthorized response. You need to re-login then and repeat your request.

In the rest of this guide, we assume that you have a valid session before you call any end-point mentioned.

### Listing VMs in inventory \(including already non-existing\)

If you have a dedicated view of the VMs that are available for restore you need call: `/virtual-machines`. This will retrieve all VMs visible by vProtect. `GUID` is the ID that you should refer to when invoking any operation on the VM. `UUID` is the ID that your infrastructure uses to identify objects.

For a multi-tenant environment make sure to filter out VMs on your side according to your ACLs or ownership of the VMs. For OpenStack vProtect records project ID and allows to use it to filter VMs like this: `/virtual-machines?tenantid={PROJECTID}`

### Getting VM details

You may want to show VM details to the end-user. Call `GET /virtual-machines/{guid}`. Some useful information includes assigned policies, protection status or last backup sizes and timestamps.

### Performing backup of a VM on the list

Backup consists of 2 phases in vProtect - export and store. End-user will initiate "backup", but your system should create an "export" task. Store task will be created automatically once export succeeds. To create an export task you need to provide the following information by the `POST /tasks/export` endpoint:

* `windowStart` and `windowEnd` - start and end of a time window for an export task - task will fail if it is not started within this time range; both values provided as UNIX time in milliseconds
* `priority` - 0-100 - higher priority tasks are executed first; 50 by default
* `backupType` - `FULL` or `INCREMENTAL`; note that incremental backups are supported only for some platforms and they require at least one schedule of type incremental assigned to the policy that VM uses; if snapshot for incremental backup is not found, a full backup will be done instead
* `backupDestination` - provided as `NameAndGuid` - target where the backup is going to be stored
* `protectedEntities` - collection of `NameAndGuid` referring to VMs that you want to backup - vProtect will create one export task for each referred VM

In the response, you'll receive task details, and you may want to record `GUID` to monitor later it's progress or status. A new `backup` entry is going to be created automatically - you may also want to record this number if you want to present its details.

### Progress monitoring of each operation

You can show the status and progress of each operation by calling `GET /tasks` \(retrieves all tasks\) and filtering the results or monitoring a particular task by calling `GET /tasks/{guid}`. There are also several useful query parameters that you can use to retrieve a filtered list:

* `protectedEntity` - GUID of VM that you refer to
* `backup` - GUID of a backup
* `schedule` - GUID of a schedule that invoked this task
* `state` - task state as `EnumNameAndDescription`:  `QUEUED`, `RUNNING`, `FINISHED`, `FAILED`, `CANCELLED`
* `type` - task type as `EnumNameAndDescription`: `INDEX`, `EXPORT`, `STORE`, `RESTORE`, `OLD_BACKUPS_REMOVAL`, `OLD_SNAPSHOTS_REMOVAL`, `IMPORT`, `MOUNT`, `UNMOUNT`, `DELETE`, `SNAPSHOT`, `SNAPSHOT_REVERSION`
* `tenantId` - to filter out tasks only belonging to VMs owned by a particular tenantID \(currently OpenStack only\)

### Browsing backup history

You can retrieve backup history including statuses, sizes and time stats for each backup by calling `GET /backups/?protected-entity={guid}`, where you provide `guid` of your VM.

### Restoring specified VM

Similar to backup, restore typically consists of several tasks. If you restore VM to the file system on the node, then it is just one task: `restore`. However, you usually want the user to restore and import VM automatically to the virtualization platform or mount it for file-level restore.

Let's focus on the restore with import case. You need to submit a task to `POST /tasks/restore-and-import` endpoint and provide:

* `backup` - GUID of a backup to be restored
* `hypervisor` or `hypervisorManager` - specify either one or the other - your target HV or HV manager depending on the virtualization platform
* `restoredPeName` - optional name of a restored VM
* `restoreStorageId` - some virtualization platforms require this to select storage to which VM has to be restored;
* `restoreClusterId` - some virtualization platforms require this to select the cluster to which VM has to be restored;
* `restoreProject` - some virtualization platforms require this to select a project to which VM has to be restored
* `dataCenter` - some virtualization platforms require this to select datacenter to which VM has to be restored; this is a `DataCenterDTO` \(currently having only `name` property\)

Once the restore task completes, an import task will be created. Monitor tasks to show user current progress.

### VM backup policy and schedule management

In order to allow users to have automatic, scheduled backup you need to create a schedule and policy and make sure that both VMs and schedules are assigned to the policy. Policy can have multiple schedules. VM can have exactly one backup policy assigned. Schedules are not assigned directly to the schedules - they are actually a part of a Policy Rule. And can be assigned to multiple rules, however in an external system this maybe not convenient and we recommend using dedicated schedules for each policy. Currently, vProtect supports one rule per policy, but from API perspective - there is a collection of rules in the policy.

Schedules can be active or not, they have to specify which type of backup needs to be done, and when. Policies specify options to automatically assign VMs based on certain criteria. It is not recommended to expose these criteria to the end-user, as currently multi-tenancy support doesn't cover this case.

When you're exposing schedules and policies you may want to filter only these owned by a particular tenant. When you create/update them you need to provide additional `tenantId` to be able later to use it when listing objects.

**Listing schedules**

To list schedules and retrieve basic info use `GET /schedules` with optional query param `tenantid`.

**Creating schedules**

To create schedule use `POST /schedules` endpoint and provide the following information:

* `name` - has to be globally unique, however you can handle uniqueness on your site or generate names if you don't need to present them to end-user
* `backupType` - `EnumNameAndDescription` - type of backup to perform: `FULL` or `INCREMENTAL`
* `type` - `EnumNameAndDescription` - for VM backup it is `VM_BACKUP` \(other options are `APP_BACKUP` and `SNAPSHOT`\); this type must match policy type
* `executionType` - `EnumNameAndDescription` - schedules can be executed at given `TIME` \(based on `hour` field\) or on `INTERVAL` basis
* `hour` - time when schedule should be invoked - it is time offset from UTC midnight in milliseconds, i.e. 3600 means 1:00 am UTC
* `active` - boolean flag to activate or deactivate schedule
* `startWindowLength` - used to assign window end to export tasks in milliseconds \(which will be set to `hour` + `startWindowLength`\)
* `daysOfWeek` - collection of `EnumNameAndDescription` - days of week `MONDAY`, ..., `SUNDAY` when schedule needs to be run
* `months` - collection of `EnumNameAndDescription` - days of week `JANUARY`, ..., `DECEMBER` when schedule needs to be run; if empty - only during specified months schedule is executed 
* `dayOfWeekOccurrences` - collection of `EnumNameAndDescription` - days of week occurrences `FIRST_IN_MONTH`, `SECOND_IN_MONTH`, `THIRD_IN_MONTH`, `FOURTH_IN_MONTH`, `LAST_IN_MONTH`, when schedule needs to be run; if empty - only during first, ..., last occurrence of specified days of week schedule is executed
* `rules` - collection of `NameAndGuid` - policy rules to which assign schedule to
* `interval` - object containing `startHour`, `endHour` \(time offset from UTC midnight in milliseconds\) and `frequency` \(also in milliseconds\) 
* `tenantId` - string identifying tenant to which assign the schedule - this is used only by 3rd party system to filter out listing

**Getting schedule details**

To get schedule details use `GET /schedules/{guid}`.

**Updating schedule**

To update the schedule use `PUT /schedules/{guid}` endpoint and provide the same information as in the creation request.

**Deleting schedule**

To delete the schedule use `DELETE /schedules/{guid}`.

**Listing VM backup policies**

To list policies and retrieve basic info use `GET /policies/vm-backup` with optional query param `tenantid`.

**Creating VM backup policy**

Policy creation is a 2 step process. It requires to create policy itself and then setting rule set on the policy. This ruleset currently must be a 1 element set.

To create schedule use `POST /policies/vm-backup` endpoint and provide the following information:

* `name` - has to be globally unique, however you can handle uniqueness on your site or generate names if you don't need to present them to end-user\* `priority` - priority assigned to backup tasks \(0-100\)
* `autoRemoveNonPresent` - boolean flag to automatically remove from policy non-existing VMs
* `autoAssignSettings` - object specifying details for VM auto-assignment mechanism - not supported in multi-tenant environment; you need to set `mode` variable of this object to `DISABLED`
* `vms` - collection of `NameAndGuid` containing GUIDs of VMs to be assigned to the policy
* `tenantId` - string identifying tenant to which assign the policy - this is used only by 3rd party system to filter out listing

You'll receive details including GUID of a newly created policy. The second step is to create a rule and assign it to the policy. Use `POST /rules/vm-backup` with the following information:

* `name` - we recommend to generate it if not used by the user
* `schedules` - collection of `NameAndGuid` you want to be assigned to this policy
* `policy` - `NameAndGuid` of a policy that you want this rule to be assigned to.

**Getting VM backup policy details**

To get policy details use `GET /policies/vm-backup/{guid}`.

**Updating VM backup policy**

To update policy use `PUT /policies/vm-backup/{guid}` endpoint and provide the same information as in the creation request. Keep in mind, that you may need to update rules as well. You can either use `DELETE /rules/vm-backup/{guid}` and later `POST /rules/vm-backup` and re-create the policy rule or use `PUT /rules/vm-backup/{guid}` to update specific settings of a specific rule. GUID of a rule can be found in policy details \(`rules` field\).

**Deleting VM backup policy**

To delete policy use `DELETE /policies/vm-backup/{guid}`. Policy rules will be removed automatically.

