# Troubleshooting

Storware vProtect log directory located in `/opt/vprotect/logs` is the first place to check for reason of errors. All of log files are also accesible from `Logs` section in web UI.

CLI interface records messages in `vprotect_client.log` files under the node subdirectory. In the same directory you can also find `vprotect_daemon.log` files which contain all engine messages. 

Web UI has 2 directories where it stores log files:

* `appserver` which has all messages comming from application server which hosts web UI
* `api` which has all messages related to the API (and Web UI)

Aditionally TSM (Spectrum Protect related errors can also be found in dsierror.log in the `/opt/vprotect` directory.
Common issues:1. UI dissappears after a while (server responds with 404 error):
  * Red Hat based distributions have build-in mechanism to clean up tempoarary directory that may remove files related to vProtect UI.
  * Make sure that `/usr/lib/tmpfiles.d/tmp.conf` file contains the following lines:

    ```
x /tmp/payaramicro*tmp
x /tmp/javamelody
    ```  * This is configured during installation since v3.4 - update from older versions requires the following commands to be executed:

    ```
tmp=/usr/lib/tmpfiles.d/tmp.conf
grep -q -F 'x /tmp/payaramicro*tmp' $tmp || echo 'x /tmp/payaramicro*tmp' >> $tmp
grep -q -F "x /tmp/javamelody" $tmp  || echo "x /tmp/javamelody" >> $tmp
    ```
    1. Cannot access Web UI (no response returned from the server)
  * make sure `vprotect-server` service is running
  * make sure firewall 8181 (or 443) ports are opened1. Index or backup tasks fail:  * the most common error is a typo in hypervisor address or credentials 
  * use hypervisor module to modify hypervisor address or credentials1.	Backup/restore fails on the export task
  * please check node log files
  * IBM Spectrum protect provides numeric code in the backup status - please refer to `Client Messages and Application Programming Interface Return Codes` for the version of TSM (Spectrum Protect) that you’re using. For version 7.1.2 you can use the following [link](http://www-01.ibm.com/support/knowledgecenter/SSGSG7_7.1.2/com.ibm.itsm.msgs.client.doc/b_msgs_client.pdf)
1. Operation fails because of privileges:
  * make sure that location where you mount backup belongs to user `vprotect`
  * make sure your staging belongs to user `vprotect`
  * for RHV/oVirt/Oracle VM make sure NFS exports are owned by user `vprotect` so that all files are accessible both by vProtect and HV manager
1. RHV/oVirt - export is OK, but fails with file not found during store
  * if you're using multiple datacenters make sure to create separate subdirectories in your staging for each DC and create exports on the subdirectories level (1 export corresponds to 1 export domain in RHV/oVirt)
1. Backup not found error for ISP backup destination:
  * make sure you have time in sync and provided correct time zone that TSM server is using
1. Tasks are not executed
  * make sure node is running in `Nodes` section in UI
  * start node if necessary
  * if it fails to start check node log files for further messages
  * if the URL to the server has changed it is necessary to update it using `vprotect node -e` command to re-register nodeTo verify if services are running you can use:

* `systemctl status vprotect-server` for Server
* `systemctl status vprotect-node` for Node or `vprotect status`