# Quick Install \(All-In-One\)

vProtect can be easily installed on a single box quickly. This approach installs a server and node on the same box and generates SSL certificate based on the hostname where it is installed. It corresponds to Ansible deployment with node and server roles installed to the same host. The end result should be the same as RPM-based installation without a staging setup. Configuration \(such as backup destination definition or hypervisor connectivity\) still needs to be done after installation. You also can add more nodes in the future if necessary.

Here are just a few steps that need to be done:

* Install **CentOS or RHEL 8** \(optionally you can use version 7\) **minimal** with **4 GB** of RAM and **2 vCPUs** and some storage for staging space and optionally a backup destination:
  * use the first disk for operating system
  * add a secondary disk, i.e. **200GB - 1 TB** \(depending on the size of your VMs that you want to backup\) - leave it empty, you'll initialize this space later
* log in as **root** over **SSH** to your machine, where you want to install it
* **RHEL** requires active subscription
* copy-and-paste this command and press ENTER:

  ```text
  bash < <(curl -s http://repo.storware.eu/vprotect/vprotect-local-install.sh)
  ```

Now you should be able to log in to vProtect Server using `https://IP_OF_YOUR_MACHINE` with local node registered and running. By default vProtect has one admin account - `admin` with password `vPr0tect` \(with zero\).

Remember to prepare your staging space as described in [Staging space configuration](common-tasks/staging-space-configuration.md).

Now please proceed with [Initial configuration](initial-configuration.md) instructions, as you need to configure access to the hypervisors and backup destinations.

