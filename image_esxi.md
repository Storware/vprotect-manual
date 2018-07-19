# vProtect deployment from image on VMware ESXi

   Log in to VMware vCenter/ESXi then click on "Create/Register VM". 
   ![](images/Images_ESXi_01.png)
   
   Select "Deploy a virtual machine from an OVF/OVA file".
   ![](images/Images_ESXi_02.png)
   
   Enter a name for the virtual machine, and select ova file.
   ![](images/Images_ESXi_03.png)

   Select storage.
   ![](images/Images_ESXi_04.png)
   

   Detailed deployment process is described on VMware site:
   https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-AFEDC48B-C96F-4088-9C1F-4F0A30E965DE.html
   
   After import image to enviroinment set IP addresation, run nmtui > "Edit a connection".
   Select network interface, and edit it network settings.
