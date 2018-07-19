# vProtect Virtual Machine deployment on Citrix XenServer

Log in to Citrix XenServer, and import downloaded xva image:  
![](../.gitbook/assets/images_citrix_01.png)

Select vProtect.xva file:  
![](../.gitbook/assets/images_citrix_02.png)

Select pool, or XenServer host:  
![](../.gitbook/assets/images_citrix_03.png)

Select storage:  
![](../.gitbook/assets/images_citrix_04.png)

Select network:  
![](../.gitbook/assets/images_citrix_05.png)

Don't check "Start VM\(s\) after import", and finish import:  
![](../.gitbook/assets/images_citrix_06.png)

When virtual appliance with vProtect was imported select second disk "vProtect-storage", and go to "Properties".  
![](../.gitbook/assets/images_citrix_07.png)

Increase disk size, for vprotect staging space.  
![](../.gitbook/assets/images_citrix_08.png)

Start vProtect virtual machine, login, and incrase disk size:

```text
   pvresize /dev/xvdb
   lvextend /dev/mapper/vg_vprotectdata-lv_vprotectdata -l+100%FREE
   xfs_growfs /dev/mapper/vg_vprotectdata-lv_vprotectdata
```

After import image to enviroinment set IP addresation, run nmtui &gt; "Edit a connection".  
Select network interface, and edit it network settings.

