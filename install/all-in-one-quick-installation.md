# All-in-one quick installation

vProtect can be easily installed on a single box quickly. You just need few steps:

* Install **CentOS minimal** with **4 GB** of RAM and **2 vCPUs** and some storage for staging space and optionally a backup destination:
  * use first disk for operating system
  * add a secondary disk, i.e. **200GB - 1 TB** \(depending on the size your VMs that you want to backup\) - leave it empty, you'll initialize this space later
* log in as **root** over **SSH** to your machine, where you want to install it
* copy-and-paste this command and press ENTER:`bash <(curl -s ftp://ftp.storware.eu/vprotect-local-install.sh)`

Now you should be able to log in to vProtect Server using `https://IP_OF_YOUR_MACHINE` with local node registered and running.

Remember to prepare your staging space as described in [Staging space configuration](staging-space-configuration.md).

Now please proceed with [Initial configuration](../initial_config/) instructions, as you need to configure access to the hypervisors and backup destinations. 
