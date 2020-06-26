# How to enable vProtect DEBUG mode

Quite often support will ask for debug logs for more in-depth troubleshooting. In order to do that please follow the steps below.

## vProtect Node

1. Edit `/opt/vprotect/log4j2-node.xml` change `INFO` to `DEBUG` in `<Property name="logLevel">...</Property>`  tag
2. Restart vProtect-node service:

   `systemctl restart vprotect-node`

3. Proceed with operations that need to be logged.
4. Then collect logs from `/opt/vprotect/logs/<NODE_NAME>` directory.

## **vProtect Server**

1. Edit `/opt/vprotect/log4j2-server.xml` change `INFO` to `DEBUG` in `<Root level="...">` tag
2. Restart vProtect-server service:

   `systemctl restart vprotect-server`

3. Proceed with operations that need to be logged.
4. Then collect logs from `/opt/vprotect/logs/api` and `/opt/vprotect/logs/appserver` directories.

