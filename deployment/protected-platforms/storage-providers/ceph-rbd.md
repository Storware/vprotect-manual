# Ceph RBD

## General

In order to connect to Ceph RBD you need to provide keyring and configuration files. Ceph RBD storage provider should detect volumes and pools in the environment and allow you to assign backup policies. vProtect uses the RBD-NBD approach to mount remote RBD snapshot over NBD and read data.

**Note:**

* vProtect needs access to the monitors specified in the Ceph configuration file.
* When creating Ceph RBD storage provider for OpenStack environment only the credentials specified in the storage provider form are used by the OpenStack backup process - actual technique \(RBD-NBD mount or cinder in disk-attachment strategy\) and **node** to connect and backup volumes depends on the OpenStack hypervisor manager settings, not in the storage provider's

## Example

Please complete the following steps to add the Ceph RBD storage provider:

* vProtect Node supports Ceph RBD and for that, you will need to install ceph libraries:
  * On **vProtect Node** enable required repositories:

For vProtect node installed on RHEL7:

```text
sudo subscription-manager repo --enable=rhel-7-server-rhceph-4-tools-rpms
```

For vProtect node installed on RHEL8:

```text
sudo subscription-manager repo --enable=rhceph-4-tools-for-rhel-8-x86_64-rpms
```

For vProtect node installed on CentOS7:

{% hint style="info" %}
sudo yum install epel-release  
sudo rpm --import '[https://download.ceph.com/keys/release.asc](https://download.ceph.com/keys/release.asc)'  
sudo yum install [https://download.ceph.com/rpm-octopus/el7/noarch/ceph-release-1-1.el7.noarch.rpm](https://download.ceph.com/rpm-octopus/el7/noarch/ceph-release-1-1.el7.noarch.rpm)
{% endhint %}

For vProtect node installed on CentOS8:

{% hint style="info" %}
sudo yum install epel-release  
sudo rpm --import '[https://download.ceph.com/keys/release.asc](https://download.ceph.com/keys/release.asc)'  
sudo yum install [https://download.ceph.com/rpm-octopus/el8/noarch/ceph-release-1-1.el8.noarch.rpm](https://download.ceph.com/rpm-octopus/el8/noarch/ceph-release-1-1.el8.noarch.rpm)
{% endhint %}

* Install rbd-nbd and ceph-common package, with all dependencies:

  ```text
  yum install rbd-nbd ceph-common
  ```

* Go to `Storage` -&gt; `Infrastructure` and add click `Add Storage Provider`
* Choose `Ceph RBD` as a type and select node responsible for backup operations
* Provide `Ceph keyring file contents` which is contents of your keyring file from Cinder host - `/etc/ceph/ceph.client.admin.keyring`, example:  
  **Note:** _Remember, both contents need to end with the new line sign._

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
    mon host = ceph-mon.domain.local
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

* Click `Save` - now you can initiate inventory synchronization \(pop-up message\) to collect information about available volumes and pools
  * later you can use `Inventory Synchronization` button on the right of the newly created provider on the list.
* Your volumes will appear in the`Instances` section in the submenu on the left, from which you can initiate backup/restore/mount tasks or view volume backup history and their details.

