# IBM Spectrum Protect

1. Register the ISP node on your ISP server.
   * Log in to your ISP server and prepare a policy set for vProtect backups.

     ```text
      Def dom vprotect
      Def pol vprotect vprotect
      Def mgmt vprotect vprotect vprotect
      Def co vprotect vprotect vprotect dest=<StoragePoolName> vere=nolimit verd=nolimit rete=nolimit reto=nolimit
      Assign defmgmt vprotect vprotect vprotect
      Activate pol vprotect vprotect
     ```

   * Register the node on the ISP server:

     ```text
     register node vprotect_proxy <SecretPassword> dom=<VMdomain> maxnummp=10 passe=0 dedup=cli backdel=yes
     ```
2. Use a script to automatically download and install the ISP API and client.

   ```text
   /opt/vprotect/scripts/setup_isp.sh
   ```

   * Make sure to type the IP address of your ISP server, TCP port and node name correctly. For example:

![](../../../.gitbook/assets/enterprise-backup-providers-ibm-spectrum-protect-install-script%20%282%29%20%282%29%20%282%29%20%282%29.png)

