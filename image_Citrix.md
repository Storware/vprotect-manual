# vProtect deployment from image on Citrix XenServer

   Log in to Citrix XenServer, and import downloaded xva image:  
   ![](images/Images_Citrix_01.png)
   
   Select vProtect.xva file:  
   ![](images/Images_Citrix_02.png)
   
   Select pool, or XenServer host:  
   ![](images/Images_Citrix_03.png)
   
   Select storage:  
   ![](images/Images_Citrix_04.png)
   
   Select network:  
   ![](images/Images_Citrix_05.png)
   
   Don't check "Start VM(s) after import", and finish import:  
   ![](images/Images_Citrix_06.png)

   When virtual appliance with vProtect was imported select second disk "vProtect-storage", and go to "Properties".  
   ![](images/Images_Citrix_07.png)
   
   Increase disk size, for vprotect staging space.  
   ![](images/Images_Citrix_08.png)
   
   Start vProtect virtual machine, login, and incrase disk size:  
   ```
   pvresize /dev/xvdb
   lvextend /dev/mapper/vg_vprotectdata-lv_vprotectdata -l+100%FREE
   xfs_growfs /dev/mapper/vg_vprotectdata-lv_vprotectdata
   ```
   
   After import image to enviroinment set IP addresation, run nmtui > "Edit a connection".  
   Select network interface, and edit it network settings.
