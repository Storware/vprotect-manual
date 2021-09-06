# Quick Install \(All-In-One\)

vProtect can be easily installed on a single box quickly. This approach installs a server and node on the same box and generates an SSL certificate based on the host name where it is installed. This corresponds to Ansible deployment, with the node and server roles installed on the same host. The end result should be the same as an RPM-based installation without staging setup. Configuration \(such as backup destination definition or hypervisor connectivity\) still needs to be done after installation. You can also add more nodes in the future if necessary.

Here are just a few steps that need to be completed:

* Install **CentOS or RHEL 8** \(optionally you can use version 7\) **minimal** with 8 **GB** of RAM and 4 **vCPUs** and some storage for staging space and optionally a backup destination:
  * use the first disk for the operating system
  * add a secondary disk, i.e. **200GB - 1 TB** \(depending on the size of your VMs that you want to backup\) - leave it empty, you'll initialize this space later
* Log in as **root** over **SSH** to the machine where you want to install it
* **RHEL 8** requires an active subscription
* copy-and-paste this command and press ENTER:

  ```text
  bash < <(curl -s http://repo.storware.eu/vprotect/vprotect-local-install.sh)
  ```

Now you should be able to log in to the vProtect Server using `https://IP_OF_YOUR_MACHINE` with the local node registered and running. By default, vProtect has one admin account - `admin` with the password `vPr0tect` \(with a zero\).

Remember to prepare your staging space as described in the [Staging space configuration](common-tasks/staging-space-configuration.md).

Now proceed with the [Initial configuration](initial-configuration.md) instructions, to configure access to the hypervisors and backup destinations.

