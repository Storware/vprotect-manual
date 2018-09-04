# vProtect Virtual Machine deployment on VMware

Log in to VMware vCenter/ESXi then click on "Create/Register VM".

![](../.gitbook/assets/images_esxi_01%20%281%29.png)

Select "Deploy a virtual machine from an OVF/OVA file".

![](../.gitbook/assets/images_esxi_02.png)

Enter a name for the virtual machine, and select ova file.

![](../.gitbook/assets/images_esxi_03.png)

Select storage.

![](../.gitbook/assets/images_esxi_04%20%281%29.png)

Detailed deployment process is described on VMware site: [https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm\_admin.doc/GUID-AFEDC48B-C96F-4088-9C1F-4F0A30E965DE.html](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-AFEDC48B-C96F-4088-9C1F-4F0A30E965DE.html)

After import image to enviroinment set IP addresation, run nmtui &gt; "Edit a connection". Select network interface, and edit it network settings.

You can find login credentials [here](./#default-login-and-password).

