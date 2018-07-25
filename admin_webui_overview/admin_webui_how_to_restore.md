# How to restore

Open "VIRTUAL MACHINES" tab, and click on ![](../.gitbook/assets/admin_webui_how_restore_icon_restore%20%281%29.png) to restore one VM. 

![](../.gitbook/assets/admin_webui_how_restore_select_one.png)

Select date/time of the backup to restore.

a\) Restore to filesystem Select node to proceed restore job, and type correct "Restore path", default path is the same like export path. Restore to filesystem option is available to all hypervisors. 

![](../.gitbook/assets/admin_webui_how_restore_filesystem.png)

b\) Restore to hypervisor \(or hypervisor manager\). Select destination hypervisor, and ID/name of the storage, and click ![](../.gitbook/assets/admin_webui_how_restore_icon_blue_restore%20%281%29.png). Restore to hypervisor is available only for Citrix XenServer, Nutanix and RHV/oVirt \(API v4\) modes. Other platforms require native management interface to import VM image to the environment \(you just need to restore files into the export path\).

![](../.gitbook/assets/zrzut-ekranu-2018-07-25-o-10.47.01%20%281%29.png)

**Notice:** every platform has some restrictions imposed on the VM name, such as length or characters that can be used. Please verify check these limits before restoring with a custom name. ****

**Notice:** KVM VMs can be imported to hypervisor on the same type of storage as they have been using during the backup.

* QCOW2-based VMs require Storage ID equal to path, where QCOW2 disks should be restored, such as `/usr/lib/libvirt/images`
* LVM-based VMs required VG name to be provided - either manually or selected from the list

