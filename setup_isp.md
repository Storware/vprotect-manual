# IBM Spectrum Protect setup

1. Rgister ISP node on your ISP server
  * Log in to your ISP server and prepare policy set for vProtect backups.

	  ```
	  Def dom vprotect
	  Def pol vprotect vprotect
	  Def mgmt vprotect vprotect vprotect
	  Def co vprotect vprotect vprotect dest=<StoragePoolName> vere=nolimit verd=nolimit rete=nolimit reto=nolimit
	  Assign defmgmt vprotect vprotect vprotect
	  Activate pol vprotect vprotect
	  ```

  * Register node on ISP server:

	  ```
register node vprotect_proxy <SecretPassword> dom=<VMdomain> maxnummp=10 passe=0 dedup=cli backdel=yes
	  ```

2. Use script to automatically download and install ISP API and client
   
   ```
/opt/vprotect/scripts/setup_isp.sh
   ```

  * Make sure to type IP address of your ISP server, TCP port, node name correctly:
  * Example:

	  ![](images/setup_isp.png)

