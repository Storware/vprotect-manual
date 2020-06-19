# OpenStack UI Plugin

## OpenStack integration setup

Download add-on from FTP. Extract the provided archive on your Horizon host and execute `python install.py VPROTECT_API_URL USER PASSWORD`

**Example:** `python install.py http://localhost:8080/api admin vPr0tect`.

**Note:** you need to restart your Horizon HTTP server after this

Above-mentioned script will copy plug-in files to the following folders:

* `/usr/share/openstack-dashboard/openstack_dashboard/dashboards/vprotect` - plugin files
* `/usr/share/openstack-dashboard/openstack_dashboard/enabled` - file to enable plugin

In order to **uninstall** it remove `vprotect` subfolder and `enabled/_50_vprotect.py` file and restart your Horizon HTTP server.

![](https://github.com/backupmonster/storware-vprotect-manual/tree/31778b5e60e67956cc3fb965d118537bb2d2be7e/.gitbook/assets/openstack-ui-plugin.png)

