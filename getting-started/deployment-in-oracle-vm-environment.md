# Deployment in Oracle VM environment

Oracle VM environment requires you to create storage used for VM export. Export storage repository should accessible also by vProtect Node in its staging directory. This implies that storage space doesn't have to be exported by vProtect Node - it can be mounted from external source. The only requirement is to have it visible from both OVM hosts and Node itself. Keep in mind that ownership of the files on the share should allow both vProtect and OVM to read and write files. Please refer to [Oracle VM setup](../initial_config/virtualization-platforms/setup_ovm.md) for details.

![](../.gitbook/assets/ovm.png)

