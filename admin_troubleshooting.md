# Troubleshooting

Storware vProtect log directory located in `/opt/vprotect/logs` is the first place to check for reason of errors. All of log files are also accesible from `Logs` section in web UI.

CLI interface records messages in `vprotect_client.log` files under the node subdirectory. In the same directory you can also find `vprotect_daemon.log` files which contain all engine messages.

Web UI has 2 directories where it stores log files:

* `appserver` which has all messages coming from application server which hosts web UI
* `api` which has all messages related to the API \(and Web UI\)

Additionally TSM \(Spectrum Protect related errors can also be found in dsierror.log in the `/opt/vprotect` directory.

To verify if services are running you can use:

* `systemctl status vprotect-server` for Server
* `systemctl status vprotect-node` for Node or `vprotect status`

## Typical issues

Typical issues can be found in the Knowledge Base: [https://storware.atlassian.net/wiki/spaces/VP/pages](https://storware.atlassian.net/wiki/spaces/VP/pages)

## Collecting debug logs

Quite often Storware support will ask for debug logs for more in-depth troubleshooting. In order to do that please follow steps below.

### vProtect Node

1. Stop vprotect-node service:

   `systemctl stop vprotect-node`

2.  * **Before 3.8.1-18** - edit `/usr/bin/vprotect` file - there is a `LOG_LEVEL=...` variable and it should look like this `LOG_LEVEL=DEBUG` and save the file.
   * **Since 3.8.1-18** - edit `/opt/vprotect/log4j2-node.xml` change `INFO` to `DEBUG` in `<Property name="logLevel">...</Property>`  tag
3. Start vprotect-node service:

   `systemctl start vprotect-node`

4. Proceed with operations that need to be logged.
5. Then collect logs from `/opt/vprotect/logs/<NODE_NAME>` directory.

### **vProtect Server**

1. Stop vprotect-server service:

   `systemctl stop vprotect-server`

2.  * **Before 3.8.1-18** - rename `/opt/vprotect/log4j2.xml.sample` file to `/opt/vprotect/log4j2.xml`  - it is a logger config for vProtect Server and it already has debug enabled.
   * **Since 3.8.1-18** - edit `/opt/vprotect/log4j2-server.xml` change `INFO` to `DEBUG` in `<Root level="...">` tag
3. Start vprotect-server service:

   `systemctl start vprotect-node`

4. Proceed with operations that need to be logged.
5. Then collect logs from `/opt/vprotect/logs/api` and `/opt/vprotect/logs/appserver` directories.

