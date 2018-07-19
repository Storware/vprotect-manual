# Backup destinations

Backup destination management module is used to add, remove backup destination \(i.e. backup provider instance with retention settings\). Backup destination must be assigned the group for scheduler to know where to put backups during automatic backup. For backup on-demand you just need to specify which backup destination should be used.

Backup destinations are assigned to the node configuration. This means that nodes using such configuration are configured to store backups in the selected backup destinations.

**Note** that backup task will fail if it is not able to assign node bacause given BD is not assigned to the node config of the node that is required be be used for given hypervisor.

To manage backup destinations in the system use `vprotect bd` sub-command.

```text
[root@vProtect3 ~]# vprotect bd
Incorrect syntax: Missing required option: [-c Create backup destination (type = ["filesystem", "netbackup", "networker", "s3", "swift", "isp"]), -s Modify backup destination, -d Delete backup destination, -D Remove old backups, -g Show backup destination details, -l List backup destinations]

usage: bd -c <NAME> <TYPE> | -d <GUID> | -D <BD_GUID,BD_GUID,...> | -g <GUID> | -l | -s <GUID> <PROPERTY_NO.> <VALUE>
Backup destination management
 -c,--create <NAME> <TYPE>                       Create backup destination (type = ["filesystem", "netbackup", "networker", "s3", "swift", "isp"])
 -d,--delete <GUID>                              Delete backup destination
 -D,--remove-old-backups <BD_GUID,BD_GUID,...>   Remove old backups
 -g,--details <GUID>                             Show backup destination details
 -l,--list                                       List backup destinations
 -s,--set <GUID> <PROPERTY_NO.> <VALUE>          Modify backup destination
```

## Setting up parameters for backup destinations

In general `vprotect bd -s <GUID> <PROPERTY_NO.> <VALUE>` command sets value of property with the given number of BD with `GUID`. Property numbers are returned in detailed view of each BD \(fields obviously are different for each backup provider\). After you create backup destination show default values with `-g`. Then use property numbers \(first number in the property/value line of the detailed view\).

Retention time settings are in interpreted in days \(just give number without any additional suffixes\).

There are however some mode/type fields which require string to be typed in correct format:

1. Amazon S3 - `Backup mode` - must be either:
   * `SINGLE_BUCKET` - single bucket for all VMs
   * `ONE_BUCKET_PER_VM` - separate bucket for each VM \(note that default limit is 100 buckets in Amazon S3\)
2. Swift - `Auth method` - authentication method used to authenticate Swift BD:
   * `BASIC`
   * `TEMPAUTH`
   * `KEYSTONE`
3. Time zones must be specified in the format shown in this table: [List of tz database time zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)

## Examples

* To list all backup destinations

  ```text
  vprotect bd -l
  ```

* To show details of the given HV manager \(by GUID\) - **note** that each BD has different set of fields - numbers at the beginning of a line indicate an indentifier of a field you want to set from CLI:

  ```text
  [root@vProtect3 ~]# vprotect bd -g f27b6018-ce1c-4d67-991e-31d9f4f6662b
  Property                                   Value                                 
  -----------------------------------------  ------------------------------------  
  GUID                                       f27b6018-ce1c-4d67-991e-31d9f4f6662b  
  Node configs                               1  
  Type                                       S3  
  Total available space                      -  
  Total used space                           2.5 GiB  
  1. Name                                    AmazonS3  
  2. Retention (full) - keep last N backups  5  
  3. Retention (full) - keep newer than      1d   
  4. Retention (inc.) - keep last N backups  5  
  5. Retention (inc.) - keep newer than      1d   
  6. Bucket key                              vprotects3  
  7. Bucket name                             vprotects3  
  8. Endpoint URL                            -  
  9. Access key                              ***  
  10. Secret key                             ***  
  11. Backup mode                            SINGLE_BUCKET
  ```

* Remember that to use a BD you need to create a new entry of a given typ and then configure its properties \(example for IBM Spectrum Protect \[ISP\], assume that GUID returned by first command was `c1d7fa34-e558-4a1c-af1f-46e88da7bcf5`\):

  ```text
  [root@localhost vprotect]# vprotect bd -c MyTSM isp
  [root@localhost vprotect]# vprotect bd -g c1d7fa34-e558-4a1c-af1f-46e88da7bcf5
  Property                                   Value                                     
  -----------------------------------------  ----------------------------------------  
  GUID                                       c1d7fa34-e558-4a1c-af1f-46e88da7bcf5  
  Node configs                               0  
  Type                                       ISP  
  Total available space                      -  
  Total used space                           -  
  1. Name                                    MyTSM  
  2. Retention (full) - keep last N backups  5  
  3. Retention (full) - keep newer than      1d   
  4. Retention (inc.) - keep last N backups  5  
  5. Retention (inc.) - keep newer than      1d   
  6. dsm.opt file path                       /opt/tivoli/tsm/client/api/bin64/dsm.opt  
  7. Node name                               -  
  8. Node password                           ***  
  9. Time zone                               UTC  
  10. Backup progress refresh rate           20  
  11. Restore progress refresh rate          20
  ```

  Now set the properties:

  * retention - full - 4 versions / 30 days
  * retention - incremental - 28 versions / 30 days
  * ISP node name
  * ISP node password
  * ISP server time zone

  ```text
  vprotect bd -s c1d7fa34-e558-4a1c-af1f-46e88da7bcf5 2 4
  vprotect bd -s c1d7fa34-e558-4a1c-af1f-46e88da7bcf5 3 30
  vprotect bd -s c1d7fa34-e558-4a1c-af1f-46e88da7bcf5 4 28
  vprotect bd -s c1d7fa34-e558-4a1c-af1f-46e88da7bcf5 5 30
  vprotect bd -s c1d7fa34-e558-4a1c-af1f-46e88da7bcf5 7 vprotect
  vprotect bd -s c1d7fa34-e558-4a1c-af1f-46e88da7bcf5 8 vpr0tectPass
  vprotect bd -s c1d7fa34-e558-4a1c-af1f-46e88da7bcf5 9 Europe/Warsaw
  ```

