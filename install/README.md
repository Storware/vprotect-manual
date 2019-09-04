# Installation

**Notice:** you may want to watch this [video](https://www.youtube.com/watch?v=c3PnfXG5Fs4), which presents a complete vProtect setup with several virtualization platforms and backup providers.

We also encourage you to read [Getting started](../getting-started/) section if you haven't done that yet.

Please remember that you also can to the following steps with Ansible using instructions in [Installation using Ansible](installation-using-ansible.md) or even quicker with [All-in-one quick installation](all-in-one-quick-installation.md) steps.

## Prerequisites

1. Install CentOS/RHEL 7 minimal \(if you plan to use deduplication with VDO we recommend to install RHEL to have Red Hat's support available\)
2. Make sure your OS is up to date:

   ```text
   yum -y update
   ```

   If kernel is updated, then You need to reboot your operating system.

3. Install vProtect repository

   create file `/etc/yum.repos.d/vProtect.repo`:

   ```text
   # vProtect Enterprise backup solution for virtual environments repository
   # http://www.vprotect.io/
   [vprotect]
   name = vProtect
   baseurl = ftp://vprotect-repo.storware.eu/repo
   gpgcheck=0
   ```

4. Install MariaDB repository \(vProtect Server host only\)
   * generate .repo file at [MariaDB download](https://downloads.mariadb.org/mariadb/repositories) site\)

     ![](../.gitbook/assets/install_prereq-mariadb%20%281%29.png)

   * copy and paste generated repo file into `/etc/yum.repos.d/MariaDB.repo`, so it looks similar to this:

     ```text
     # MariaDB 10.3 CentOS repository list - created 2019-08-08 10:31 UTC
     # http://downloads.mariadb.org/mariadb/repositories/
     [mariadb]
     name = MariaDB
     baseurl = http://yum.mariadb.org/10.3/centos7-amd64
     gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
     gpgcheck=1
     ```

## vProtect Server installation

vProtect consists of server \(central management point with WebUI\) and one or multiple nodes \(which can be installed on the same host as server or on other machines\). First step is always to install server.

1. Install vprotect-server using YUM:

   ```text
   yum -y install vprotect-server
   ```

2. Setup DB for vProtect:

   ```text
   vprotect-server-configure
   ```

   ![](../.gitbook/assets/install_server-configure%20%281%29.png)

3. Start vProtect Server \(it can take around a minute for server to be started\):

   ```text
   systemctl start vprotect-server
   ```

4. You may need to open 8181 port on your firewall. Here is example:

   ```text
   firewall-cmd --add-port=8181/tcp --permanent
   firewall-cmd --complete-reload
   ```

   **or** if you prefer to have vProtect console running on default HTTPS port \(443\), there is a script in `/opt/vprotect/scripts` directory to setup forwarding from port 443 to 8181 for you:

   ```text
   cd /opt/vprotect/scripts
   ./ssl_port_forwarding_firewall-cmd.sh
   ```

   ![](../.gitbook/assets/install_server-firewall.png)

5. Now you should be able to log into the web console using URL: `https://VPROTECT_HOST:8181`, where `VPROTECT_HOST` is hostname or IP of your vProtect Server
   * by default vProtect has one admin account - `admin` with password `vPr0tect` \(with zero\)
6. Optionally, if you are going to connect nodes running on remote hosts, please proceed with the steps related to the vProtect Server described in section: [Enabling HTTPS connectivity for remote nodes](install_https.md)

## vProtect Node installation

vProtect Node is component that executes all tasks. It can be installed together with Server \(it is common to have 1 server and just 1 node\). More nodes can be always added later.

1. Install vprotect-node using YUM:

   ```text
   yum -y install vprotect-node
   ```

2. Prepare your staging space \(on vProtect Node host only\):
   * Please follow steps described in [Staging space configuration](staging-space-configuration.md)
   * **if your path is different than** `/vprotect_data` ****it is recommended to create a symlink `/vprotect_data` pointing to your mount point of the staging space, e.g.:

     ```text
     ln -s /mnt/staging /vprotect_data
     ```

   * Register node with `NODE_NAME` of your choice `ADMIN_USER` user name which you would like to use and URL to vProtect API and provide password when prompted:
3. * Syntax:

     ```text
     vprotect node -r NODE_NAME ADMIN_USER http(s)://VPROTECT_SERVER:PORT/api
     ```

   * If you are going to connect nodes running on remote hosts, please proceed with the steps related to the vProtect Node described in section: [Enabling HTTPS connectivity for remote nodes](install_https.md)
   * Example for default local installation - over HTTP \(port 8080\):

     ```text
     vprotect node -r node1 admin http://localhost:8080/api
     ```
4. Start vProtect Node:

   ```text
   systemctl start vprotect-node
   ```

   Now you should be able to see new entry in `Node` section of web UI with your node in RUNNING state.

5. Run script to configure OS for Node:

   ```text
   vprotect-node-configure
   ```

   ![](../.gitbook/assets/install_node-configure.png)

6. Reboot vProtect VM to apply OS-specific settings:

   ```text
   reboot
   ```

vProtect is installed - you can now proceed with the steps described in [initial configuration](../initial_config/).

