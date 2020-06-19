# Staging space configuration

vProtect node needs staging space available in `/vprotect_data` by default. It is common to just attach empty drive and mount it.

Staging space size depends on the number and size of simultaneous backups - as a rule of a thumb make it approximately equal to the number of expected simultaneous backup threads multiplied by the size of your biggest VM

## PowerProtect DD

Follow these steps to use your DD as a staging space and backup destination

* Login to PowerProtect DD and create Storage Unit for vProtect - with name i.e. `vprotectbackup`
* Assign `vprotect` user \(on DD\) to this Storage Unit
* Download BoostFS RPM to your vProtect Node machine
* Rest of the steps are on the vProtect Node machine
* Install BoostFS:

  ```text
  rpm -ivh DDBoostFS-7.0.0.0-633922.rhel.x86_64.rpm
  ```

* Save password for BoostFS \(`DD_BOOST_IP` should be replaced with IP of your DD\):

  ```text
  /opt/emc/boostfs/bin/boostfs lockbox set -d DD_BOOST_IP -u vprotect -s vprotectbackup
  ```

* Provide your DD password when prompted
* Add `/etc/fstab` entry:

  ```text
  DD_BOOST_IP:/vprotectbackup /vprotect_data boostfs defaults,_netdev,bfsopt(allow-others=true) 0 0
  ```

  **Note:** For manual, one-time mount you can run this command:

  ```text
  /opt/emc/boostfs/bin/boostfs mount -o allow-others=true -d DD_BOOST_IP -s vprotectbackup
  /vprotect_data
  ```

* Mount

  ```text
  mount -a
  ```

* Set ownership to `vprotect`user on directory `/vprotect_data`:

  ```text
  chown vprotect:vprotect -R /vprotect_data
  ```

* Confirm with `df` that your `/vprotect_data` is mounted
* Restart your `vprotect-node` service:

  ```text
  systemctl restart vprotect-node
  ```

* Remember to specify backup destination path to be a subdirectory of `/vprotect_data` if you would like to use same storage unit as a staging space and backup destination - example: `/vprotect_data/my-backups`

## Local filesystem

You also can use a plain file system for staging space \(and optionally for backup destination\). Here are steps assuming you have a local \(physical or virtual\) disk.

* List all existing disks, and find your dedicated disk \(let's say - `/dev/sdc`\):

  ```text
     [root@vProtect01 ~]# fdisk -l | grep dev
     Disk /dev/sda: 32.2 GB, 32212254720 bytes, 62914560 sectors
     /dev/sda1   *        2048     1026047      512000   83  Linux
     /dev/sda2         1026048    62914559    30944256   8e  Linux LVM
     Disk /dev/sdc: 500 GB, 17179869184 bytes, 33554432 sectors
     Disk /dev/sdb: 21.5 GB, 21474836480 bytes, 41943040 sectors
     Disk /dev/mapper/centos-root: 28.5 GB, 28462546944 bytes, 55590912 sectors
     Disk /dev/mapper/centos-swap: 3221 MB, 3221225472 bytes, 6291456 sectors
  ```

* If you have new clean disk prepare a filesystem on it:

  ```text
  mkfs.xfs -K /dev/sdc
  ```

* Test mount your existing filesystem in created directory:

  ```text
  mount /dev/sdc /vprotect_data
  ```

* Set ownership to `vprotect` user on directory `/vprotect_data`:

  ```text
  chown vprotect:vprotect -R /vprotect_data
  ```

* Add a line to `/etc/fstab` file, to automatically mount new filesystem after reboot:

  ```text
  /dev/sdc    /vprotect_data    xfs    defaults 0 0
  ```

* Mount

  ```text
  mount -a
  ```

* Confirm with `df` that your `/vprotect_data` is mounted
* Restart your `vprotect-node` service:

  ```text
  systemctl restart vprotect-node
  ```

