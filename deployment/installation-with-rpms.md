# Installation with RPMs

## Prerequisites

1. Install CentOS/RHEL 8 minimal
   * if you plan to use deduplication with VDO we recommend to use RHEL to have Red Hat's support available
   * you also can use version CentOS/RHEL 7
2. Make sure your OS is up to date:

   ```text
   dnf -y update
   ```

   If the kernel is updated, then you need to reboot your operating system.

3. Install the vProtect repository

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

4. Install the MariaDB repository \(**vProtect Server host only**\)
   * generate .repo file at [MariaDB download](https://downloads.mariadb.org/mariadb/repositories) site
   * copy and paste the generated repo file into `/etc/yum.repos.d/MariaDB.repo`, so it looks similar to this \(this one for CentOS/RHEL 8\):

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

vProtect consists of a server \(a central management point with a WebUI\) and one or multiple nodes \(which can be installed on the same host as the server or on other machines\). The first step is always to install the server.

1. Install the vprotect-server using DNF package manager:

   ```text
   dnf -y install vprotect-server
   ```

2. Set up the DB for vProtect:

   * Please provide a MariaDB password
   * Please remember to not re-run this command on running/production vProtect instance. This can cause problems with the vProtect database.

   ```text
   vprotect-server-configure
   ```

3. Start the vProtect Server \(it can take around a minute for the server to start\):

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

   **or** if you prefer to have the vProtect console running on the default HTTPS port \(443\), there is a script in the `/opt/vprotect/scripts` directory to set up forwarding from port 443 to 8181:

   ```text
   cd /opt/vprotect/scripts
   ./ssl_port_forwarding_firewall-cmd.sh
   ```

5. Now you should be able to log in to the web console using the URL: `https://VPROTECT_HOST:8181`, where `VPROTECT_HOST` is the hostname or IP of your vProtect Server
   * by default, vProtect has one admin account - `admin` with the password `vPr0tect` \(with a zero\)
6. Optionally, if you are going to connect nodes running on remote hosts, proceed with the steps related to the vProtect Server described in the section: [Enabling HTTPS connectivity for remote nodes](common-tasks/enabling-https-connectivity-for-remote-nodes.md)

## vProtect Node installation

vProtect Node is a component that executes all tasks. It can be installed together with the Server \(it is common to have 1 server and just 1 node\). More nodes can always be added later.

1. Install the vprotect-node using DNF:  
   _Remember to add our repository to host machine_

   ```text
   dnf -y install vprotect-node
   ```

2. Prepare your staging space \(on the vProtect Node host only\):
   * If you just started with vProtect, and do not know what is staging space please follow the steps described in the [Staging space configuration](common-tasks/staging-space-configuration.md)
   * **if your path is different than** `/vprotect_data` it is recommended to create a symlink `/vprotect_data` pointing to your staging space mount point, e.g.:

     ```text
     ln -s /mnt/staging /vprotect_data
     ```

   * Register the node with the `NODE_NAME` of your choice, the `ADMIN_USER` user name which you would like to use and the URL to the vProtect API, and provide a password when prompted:
3. * Syntax:

     ```text
     vprotect node -r NODE_NAME ADMIN_USER http(s)://VPROTECT_SERVER:PORT/api
     ```

   * If you are going to connect nodes running on remote hosts, please proceed with the steps related to the vProtect Node described in the section: [Enabling HTTPS connectivity for remote nodes](common-tasks/enabling-https-connectivity-for-remote-nodes.md)
   * Example for default local installation - over HTTP \(port 8080\):

     ```text
     vprotect node -r node1 admin http://localhost:8080/api
     ```
4. Start the vProtect Node:

   ```text
   systemctl start vprotect-node
   ```

   Now you should be able to see a new entry in the `Node` section of the web UI with your node in RUNNING state.

5. Run the script to configure the OS for Node, which includes changing the QEMU user/group to `vprotect`, disabling SELinux, adding vprotect to the disk group and sudoers to allow it to run privileged commands:

   ```text
   vprotect-node-configure
   ```

6. Reboot the vProtect VM to apply the OS-specific settings:

   ```text
   reboot
   ```

vProtect is installed - you can now proceed with the steps described in the [initial configuration](initial-configuration.md).

