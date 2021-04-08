# Installation with RPMs

## Prerequisites

1. Install CentOS/RHEL 8 minimal
   * if you plan to use deduplication with VDO we recommend to use RHEL to have Red Hat's support available
   * you also can use version CentOS/RHEL 7
2. Make sure your OS is up to date:

   ```text
   dnf -y update
   ```

   If the kernel is updated, then You need to reboot your operating system.

3. Install vProtect repository

   * create file `/etc/yum.repos.d/vProtect.repo`:

   ```text
   # vProtect Enterprise backup solution for virtual environments repository
   [vprotect]
   name = vProtect
   baseurl = http://repo.storware.eu/vprotect/current/el8
   gpgcheck=0
   ```

   * optionally change `el8` to `el7` for older CentOS/RHEL and `current` can also be pointed to the specific version of vProtect, i.e. `3.9.2` \(not the one that is always up to date\)
   * so here are effective URLs as examples: 
     * `http://repo.storware.eu/vprotect/current/el7`
     * `http://repo.storware.eu/vprotect/3.9.2/el8`

4. Install MariaDB repository \(**vProtect Server host only**\)
   * generate .repo file at [MariaDB download](https://downloads.mariadb.org/mariadb/repositories) site
   * copy and paste generated repo file into `/etc/yum.repos.d/MariaDB.repo`, so it looks similar to this \(this one for CentOS/RHEL 8\):

     ```text
     # MariaDB 10.4 CentOS repository list - created 2020-06-01 16:14 UTC
     # http://downloads.mariadb.org/mariadb/repositories/
     [mariadb]
     name = MariaDB
     baseurl = http://yum.mariadb.org/10.4/centos8-amd64
     module_hotfixes=1
     gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
     gpgcheck=1
     ```

## vProtect Server installation

vProtect consists of a server \(central management point with WebUI\) and one or multiple nodes \(which can be installed on the same host as a server or on other machines\). The first step is always to install a server.

1. Install vprotect-server using DNF package manager:

   ```text
   dnf -y install vprotect-server
   ```

2. Setup DB for vProtect:

   * Please provide a MariaDB password
   * Please remember to not re-run this command on running/production vProtect instance. This can cause problems with the vProtect database.

   ```text
   vprotect-server-configure
   ```

3. Start vProtect Server \(it can take around a minute for the server to be started\):

   ```text
   systemctl start vprotect-server
   ```

4. You may need to open the 8181 port \(for HTTPS, HTTP requires port 8080\) on your firewall. Here is an example:

   ```text
   firewall-cmd --add-port=8181/tcp --permanent
   firewall-cmd --complete-reload
   # To check open ports:
   firewall-cmd --list-all
   ```

   **or** if you prefer to have vProtect web console running on default HTTPS port \(443\), there is a script in `/opt/vprotect/scripts` directory to setup forwarding from port 443 to 8181 for you:

   ```text
   cd /opt/vprotect/scripts
   ./ssl_port_forwarding_firewall-cmd.sh
   ```

5. Now you should be able to log into the web console using URL: `https://VPROTECT_HOST:8181`, where `VPROTECT_HOST` is hostname or IP of your vProtect Server
   * by default, vProtect has one admin account - `admin` with password `vPr0tect` \(with zero\)
6. Optionally, if you are going to connect nodes by https, proceed with the steps related to the vProtect Server described in section: [Enabling HTTPS connectivity for nodes](common-tasks/enabling-https-connectivity-for-nodes.md)

## vProtect Node installation

vProtect Node is a component that executes all tasks. It can be installed together with a Server \(it is common to have 1 server and just 1 node\). More nodes can be always added later.

1. Install vprotect-node using DNF:  
   _Remember to add our repository to host machine_

   ```text
   dnf -y install vprotect-node
   ```

2. Prepare your staging space \(on vProtect Node host only\):
   * If you just started with vProtect, and do not know what is staging space please follow the steps described in the [Staging space configuration](common-tasks/staging-space-configuration.md)
   * **if your path is different than** `/vprotect_data` it is recommended to create a symlink `/vprotect_data` pointing to your mount point of the staging space, e.g.:

     ```text
     ln -s /mnt/staging /vprotect_data
     ```

   * Register node with `NODE_NAME` of your choice `ADMIN_USER` user name which you would like to use and URL to vProtect API and provide the password when prompted:
3. * Syntax:

     ```text
     vprotect node -r NODE_NAME ADMIN_USER http(s)://VPROTECT_SERVER:PORT/api
     ```

   * If you are going to connect nodes running on remote hosts, please proceed with the steps related to the vProtect Node described in section: [Enabling HTTPS connectivity for remote nodes](common-tasks/enabling-https-connectivity-for-nodes.md)
   * Example for default local installation - over HTTP \(port 8080\):

     ```text
     vprotect node -r node1 admin http://localhost:8080/api
     ```
4. Start vProtect Node:

   ```text
   systemctl start vprotect-node
   ```

   Now you should be able to see a new entry in `Node` section of web UI with your node in RUNNING state.

5. Run script to configure OS for Node, which includes QEMU user/group changed to `vprotect`, disabling SELinux, adding vprotect the disk group and sudoers to allow it to run privileged commands:

   ```text
   vprotect-node-configure
   ```

6. Reboot vProtect VM to apply OS-specific settings:

   ```text
   reboot
   ```

vProtect is installed - you can now proceed with the steps described in the [initial configuration](initial-configuration.md).

