# Small

* Network: minimum 1Gb/s
* Number of sites: 1
* Number of **vProtect** installations: 1 server alongside with node
* vProtect min. 2 vCPU and 4 GB RAM 
* Number of backup threads: 3-4 \(default\)
* Backup window: 8 hours
* Data to backup: up to 2.5 TB
* staging space: your\_largest\_VM \* backup threads

As mentioned above, network speed is a very important factor. When you have 1Gb/s connection between server, node and backup clients then during 8 hours of backup window we can assume that if we load the network at 70% of it is capacity we should transfer 2.5TB of data.

In this scenario **vProtect** \(server and node\) can be for example deployed as a VM using any of the installation methods.

![](../../.gitbook/assets/sizing_small.png)

