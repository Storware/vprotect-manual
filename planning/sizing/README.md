# Sizing

There are two types of people: those who are doing backups, and those who will be doing them. But just **doing** backups is not everything. To avoid unnecessary surprises the best strategy is to **plan** your backup environment/procedure **before implementing** it. In this chapter we have collected generic hints and guides which you might find useful while thinking about your **vProtect** implementation.

Of course many factors will affect your final deployment, but it is a good practice to answer the following questions:

1. **What?**
   * What is the type of data you want to protect? This may or not include applications, databases, virtual machine images, containers or a mix of those. 
   * Do you run virtualization platforms, or maybe in a cloud?
   * How big is the volume of your data, what is the number of running servers? The size of your biggest VM multiplied by the number of parallel backup threads will determine the size of [**staging space**](../../deployment/common-tasks/staging-space-configuration.md)**.**
2. **How?**
   * What is the connection speed between your servers? **vProtect** mostly utilizes network to transfer data between client and node, so it's best to have a high-speed connection between backup clients and **vProtect** nodes. 
   * How and where will you store your backups? Is it enough to use a simple \(or deduplicated\) dedicated filesystem \(separated from production environment\), or maybe you need a more efficient backup destination? If you decide to use the local filesystem as a backup destination you can as well use it as a staging space at the same time. This will greatly improve the effectiveness of deduplication and speed of moving data from staging to it is final destination.
   * If you have multiple data centers do you plan to replicate backups across them?
   * How many virtual environments do you operate? Remember that you need at least one **vProtect** node per virtual environment.
3. **When?**
   * What is the best time to take backups? The most standard approach is to have a backup window during the night \(for example 12 hours between 18:00 and 6:00\). Depending on the volume of your data, connection speed between infrastructure elements and application characteristics, you might consider adjusting it to suit your needs. You can shorten it if needed, by backing up many clients in parallel, thus generating more network load.

When you take the above questions into consideration you should have at least a good starting point to plan your backup strategy.

Based on the above, we prepared three scenarios that are typical for most use cases.

