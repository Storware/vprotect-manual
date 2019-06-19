# OpenStack setup

vProtect supports backup for KVM hypervisors with VMs using QCOW2 volumes. 

**Example:** scenario QCOW2 files residing on NFS:

You can configure the NFS volume backend here:

[https://docs.openstack.org/cinder/rocky/admin/blockstorage-nfs-backend.html](https://docs.openstack.org/cinder/rocky/admin/blockstorage-nfs-backend.html)

Make sure QCOW2 volumes are enabled. For an NFS backend, it's recommended set these values in `/etc/cinder/cinder.conf`:

`default_volume_type=nfs` `nfs_sparsed_volumes = true` `nfs_qcow2_volumes = true` `volume_driver = cinder.volume.drivers.nfs.NfsDriver` `enabled_backends = nfs`

```text
[nfs]
volume_backend_name=nfs
volume_driver = cinder.volume.drivers.nfs.NfsDriver
```

### Adding hypervisor managers

When creating the hypervisor manager, provide the following data in fields:

URL - Keystone API URL, e.g. `http://10.201.32.40:5000/v3`

Username - OpenStack user

Password - password for that user.

When you index the hypervisor manager, **make sure to provide correct SSH credentials** for each hypervisor that appeared in the Hypervisors tab. You can also use [SSH public key authentication](../../install/ssh-public-key-authentication.md).

**Note: When restoring the instances, make sure that the provided user is a member of the target tenant.**

### OpenStack integration setup

Download add-on from FTP. Extract the provided archive on your Horizon host and execute `python install.py VPROTECT_API_URL USER PASSWORD` 

**Example:** `python install.py http://localhost:8080/api admin vPr0tect`.

**Notice:** you need to restart your Horizon HTTP server after this

Above-mentioned script will copy plug-in files to the following folders:

* `/usr/share/openstack-dashboard/openstack_dashboard/dashboards/vprotect` - plugin files
* `/usr/share/openstack-dashboard/openstack_dashboard/enabled` - file to enable plugin

In order to **uninstall** it remove `vprotect` subfolder and  `enabled/_50_vprotect.py` file and restart your Horizon HTTP server.

