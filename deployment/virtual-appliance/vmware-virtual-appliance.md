# VMware Virtual Appliance

* Log in to VMware vCenter/ESXi then click on "Create/Register VM".

![](../../.gitbook/assets/virtual-appliance-vmware-esxi-01%20%282%29%20%282%29%20%282%29%20%282%29%20%282%29.png)

* Select "Deploy a virtual machine from an OVF/OVA file".

![](../../.gitbook/assets/virtual-appliance-vmware-esxi-02%20%282%29%20%282%29%20%282%29%20%282%29.png)

* Enter a name for the virtual machine, and select the ova file.

![](../../.gitbook/assets/virtual-appliance-vmware-esxi-03%20%281%29%20%282%29%20%282%29%20%282%29%20%282%29.png)

* Select the storage.

![](../../.gitbook/assets/virtual-appliance-vmware-esxi-04%20%281%29.png)

The detailed deployment process is described on the VMware site: [https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm\_admin.doc/GUID-AFEDC48B-C96F-4088-9C1F-4F0A30E965DE.html](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-AFEDC48B-C96F-4088-9C1F-4F0A30E965DE.html)

After importing the image to the environment, set the IP address and run nmtui &gt; "Edit a connection". Select the network interface and edit the network settings.

You can find the login credentials [here](./).

