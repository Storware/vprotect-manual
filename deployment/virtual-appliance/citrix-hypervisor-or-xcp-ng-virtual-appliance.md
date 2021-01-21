# Citrix Hypervisor \| XCP-ng Virtual Appliance

* Log in to Citrix XenServer, and import downloaded xva image:

![](../../.gitbook/assets/virtual-appliance-citrix-xcp_ng-01%20%281%29.png)

* Select vProtect.xva file:

![](../../.gitbook/assets/virtual-appliance-citrix-xcp_ng-02%20%282%29.png)

* Select pool, or XenServer host:

![](../../.gitbook/assets/virtual-appliance-citrix-xcp_ng-03.png)

* Select storage:

![](../../.gitbook/assets/virtual-appliance-citrix-xcp_ng-04%20%281%29.png)

* Select network:

![](../../.gitbook/assets/virtual-appliance-citrix-xcp_ng-05.png)

* Don't check "Start VM\(s\) after import", and finish import:

![](../../.gitbook/assets/virtual-appliance-citrix-xcp_ng-06.png)

* When virtual appliance with vProtect was imported select second disk "vProtect-storage", and go to "Properties".

![](../../.gitbook/assets/virtual-appliance-citrix-xcp_ng-07%20%282%29.png)

* Increase disk size for vProtect staging space.

![](../../.gitbook/assets/virtual-appliance-citrix-xcp_ng-08%20%282%29.png)

* Start vProtect virtual machine, login, and increase disk size \(in that example to 500G\):

```text
vdo growPhysical -n /dev/mapper/FileSystem
   vdo growLogical -n /dev/mapper/FileSystem --vdoLogicalSize 500G
   xfs_growfs /dev/mapper/FileSystem
```

After import image to environment set IP address, run nmtui &gt; "Edit a connection".  
Select network interface, and edit it network settings.

You can find login credentials [here](./).

