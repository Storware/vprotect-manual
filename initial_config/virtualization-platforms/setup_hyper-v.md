# Microsoft Hyper-v setup

## RHV/oVirt \(API v4\)


1. From our FTP server ftp://ftp.storware.eu/Add-ons/ download **HyperV-Agent-Installer.msi**, and **setup.exe**.
2. Put installation files to Hyper-v host.
3. Run installation from **setup.exe**.

4. Click **Next** to proceed installation.

5. Type path to install Hyper-V Agent. And accept it by click **Next**.

6. Chose Protocol to comunicate between agent, and vProtect node. And accept it by click **Next**.

7. Provide password for secure communication. And accept it by click **Next**.

8. Click **Next** to start installation.

9. If Windows UAC prompt you about installation, accept it by choose **Yes**.

10. Go to **vProtect WebUI** > **HYPERVISORS** > **Hypervisors** click on button  **+Add Hypervisor**.
11. In "Add New Hypervisor" window fill all fields:
    URL for https have port 50882, for http use port 50881.
    password - use the same what you set in installation agent process.

12. Click **Save** to finish adding your Hyper-v host.
    Repeat all steps for all Hyper-v hosts.


