# OpenStack setup

vProtect supports two backup solutions for OpenStack: 1. \#\#\#\# Disk image transfer - for KVM hypervisors with VMs using QCOW2 volumes or Ceph-based storage.

* supports incremental backup
* disk images are transferred directly from API \(no Proxy VM required\)
* **Disk attachment through Cinder**
* supports all hypervisors and storages
* no incremental backup
* proxy VM is required - used for disk attachment process.

## Disk attachment

This backup mode requires vProtect Node to be installed in one of the VMs residing on your OpenStack installation. vProtect should detect automatically the VM with vProtect during index operation.

## QCOW2 files on NFS storage

**Example:** scenario QCOW2 files residing on NFS

You can configure the NFS volume backend here:

[https://docs.openstack.org/cinder/rocky/admin/blockstorage-nfs-backend.html](https://docs.openstack.org/cinder/rocky/admin/blockstorage-nfs-backend.html)

Make sure QCOW2 volumes are enabled.

For an NFS backend, it's recommended set these values in `/etc/cinder/cinder.conf`:

`default_volume_type=nfs    
nfs_sparsed_volumes = true    
nfs_qcow2_volumes = true    
volume_driver = cinder.volume.drivers.nfs.NfsDriver    
enabled_backends = nfs`

## Ceph RBD storage

vProtect supports OpenStack with Ceph RBD volumes. Here is an example of a typical \(expected\) section that needs to be added in `cinder.conf` for Ceph in OpenStack environment:

```text
[rbd]
volume_backend_name = rbd
volume_driver = cinder.volume.drivers.rbd.RBDDriver
rbd_pool = volumes
rbd_ceph_conf = /etc/ceph/ceph.conf
rbd_flatten_volume_from_snapshot = false
rbd_max_clone_depth = 5
rbd_store_chunk_size = 4
rados_connect_timeout = -1
glance_api_version = 2
rbd_user = volumes
rbd_secret_uuid = ce6d1549-4d63-476b-afb6-88f0b196414f
```

A good article how to setup Ceph with OpenStack you can find [here](https://superuser.openstack.org/articles/ceph-as-storage-for-openstack/).

vProtect Node supports Ceph RBD and you only need to install rbd-nbd package:

1. On **vProtect Node** - add Ceph RPM repository \(like [here](https://docs.ceph.com/docs/master/install/install-storage-cluster/)\) - example `/etc/yum.repos.d/ceph.repo`

   \(for nautilus release\):

   ```text
   [ceph]
   name=Ceph packages for $basearch
   baseurl=https://download.ceph.com/rpm-nautilus/el7/$basearch
   enabled=1
   priority=2
   gpgcheck=1
   gpgkey=https://download.ceph.com/keys/release.asc
   ```

2. Install `rbd-nbd` package:

   `yum install rbd-nbd`

3. Add hypervisor manager as described [here](openstack-setup.md#adding-hypervisor-managers) and while you add them:
   * switch `Enable Ceph` toggle to on
   * provide `Ceph keyring file contents` which is contents of your keyring file from Cinder host - `/etc/ceph/ceph.client.admin.keyring`, example:

```text
     [client.admin]
     key = AQCCQG5dGKhUFBAA9G7TTQWfFXbF1ywbqpA1Vw==
     caps mds = "allow *"
     caps mgr = "allow *"
     caps mon = "allow *"
     caps osd = "allow *"
```

* provide `Ceph configuration file contents` , example:

```text
     [global]
     cluster network = 10.40.0.0/16
     fsid = cc3a4e9f-d2ca-4fec-805d-2c40605723b3
     mon host = ceph-mon.storware.local
     mon initial members = ceph-00
     osd pool default crush rule = -1
     public network = 10.40.0.0/16
     [client.images]
     keyring = /etc/ceph/ceph.client.images.keyring
     [client.volumes]
     keyring = /etc/ceph/ceph.client.volumes.keyring
     [client.nova]
     keyring = /etc/ceph/ceph.client.nova.keyring
```

1. Now you can save and sync inventory - if Ceph communication works properly you should be able to see Hypervisor Storage entries \(in Hypervisors -&gt; Storage tab\) representing your Ceph storage pools.

## Adding hypervisor managers

When creating the hypervisor manager, provide the following data in fields:

URL - Keystone API URL, e.g. `http://10.201.32.40:5000/v3`

Username - OpenStack user

Password - password for that user.

When you index the hypervisor manager, **make sure to provide correct SSH credentials** for each hypervisor that appeared in the Hypervisors tab. You can also use [SSH public key authentication](../../install/ssh-public-key-authentication.md).

**Note: When restoring the instances, make sure that the provided user is a member of the target tenant.**

## OpenStack integration setup

Download add-on from FTP. Extract the provided archive on your Horizon host and execute `python install.py VPROTECT_API_URL USER PASSWORD`

**Example:** `python install.py http://localhost:8080/api admin vPr0tect`.

**Notice:** you need to restart your Horizon HTTP server after this

Above-mentioned script will copy plug-in files to the following folders:

* `/usr/share/openstack-dashboard/openstack_dashboard/dashboards/vprotect` - plugin files
* `/usr/share/openstack-dashboard/openstack_dashboard/enabled` - file to enable plugin

In order to **uninstall** it remove `vprotect` subfolder and `enabled/_50_vprotect.py` file and restart your Horizon HTTP server.

