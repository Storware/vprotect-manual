# Troubleshooting

Storware vProtect log directory path is `/opt/vprotect/logs` - this is the first place to check for root cause of errors. All log files are also accessible from `Logs` section in web UI.

CLI interface records messages in `vprotect_client.log` files under the subdirectory with the same name as the node. In the same directory, you can also find `vprotect_daemon.log` files which contains all engine-related messages.

Web UI has 2 directories where it stores log files:

* `appserver` which has all messages coming from the application server - which hosts web UI and API
* `api` which has all messages related to - core vProtect Server application

To verify if services are running you can use:

* `systemctl status vprotect-server` for Server
* `systemctl status vprotect-node` for Node or `vprotect status`

If you don't find here the root cause of the problem you can switch vProtect to [DEBUG mode](how-switch-vprotect-to-debug-mode.md), and recreate the task to generate logs in DEBUG mode.

