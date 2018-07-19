# Setup LVM daemon on vProtect Node for disk attachment backup mode

**Notice:** this is required for **Nutanix** and **RHV v4 API** (when using disk attachment mode)

vProtect Node attaches VM disks that potentially are clones of its own (i.e. if Node deployed from template) - you need to configure LVM on the Node so that it doesn't scan for LVM volumes where disks are being attached.

1. Set the following variables in `/etc/lvm/lvm.conf` - so that only system volumes are being detected by LVM daemon (in this example `/dev/sda1`):
 
  ```
filter = [ "a|^/dev/sda1$|", "r|.*|" ]
global_filter = [ "a|^/dev/sda1$|", "r|.*|" ]
  ```

1. Check with `vgscan -vvv` that your OS volumes are still being detected:

  ```
...
        Allocated VG vg_vprotect at 0x55914f19fac0.
        Importing logical volume vg_vprotect/lv_root.
        Importing logical volume vg_vprotect/lv_swap.
...
  ```
  
1. Reboot:

  ```
reboot
  ```