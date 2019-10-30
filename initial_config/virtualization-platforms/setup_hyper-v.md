# Microsoft Hyper-v setup

## RHV/oVirt \(API v4\)

1. From our FTP server ftp://ftp.storware.eu/Add-ons/ download **HyperV-Agent-Installer.msi**, and **setup.exe**.
2. Put installation files to Hyper-v host.
3. Run installation from **setup.exe**.

![](../../.gitbook/assets/00.png)

4. Click **Next** to proceed installation.

![](../../.gitbook/assets/01.png)

5. Type path to install Hyper-V Agent. And accept it by click **Next**.

![](../../.gitbook/assets/02.png)

6. Chose Protocol to communicate between agent, and vProtect node. And accept it by click **Next**.

![](../../.gitbook/assets/03.png)

7. Provide password for secure communication. And accept it by click **Next**.

![](../../.gitbook/assets/04.png)

8. Click **Next** to start installation.

![](../../.gitbook/assets/05.png)

9. If Windows UAC prompt you about installation, accept it by choose **Yes**.

![](../../.gitbook/assets/06.png)

10. Click **Finish** to end installation.

![](../../.gitbook/assets/08.png)

11. Go to **vProtect WebUI** &gt; **HYPERVISORS** &gt; **Hypervisors** click on button **+Add Hypervisor**.

12. In "Add New Hypervisor" window fill all fields: URL for https have port 50882, for http use port 50881. password - use the same what you set in installation agent process.

![](../../.gitbook/assets/09.png)

13. Click **Save** to finish adding your Hyper-v host. Repeat all steps for all Hyper-v hosts.

