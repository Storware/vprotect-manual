# Infrastructure

This section describes how to manage Storage Providers in vProtect. Inventory that vProtect needs first to be populated. The first step is always to add a storage provider or File-system visible on the vProtect node.

![](../../.gitbook/assets/storage-providers-infrastructure.jpg)

Click Add Storage Provider to add entries

![](../../.gitbook/assets/storage-infrastructure-add-ceph-rbd.jpg)

Then synchronize inventory \(either automatically - there will be a dialog box shown just after saving the form or manually with the button on the right of storage provider\). If Inventory Synchronization tasks \(visible in the console at the bottom\) completes successfully it also proves that connection was successful, authentication is correct and all of the inventory items have been collected successfully.

Check Storage pools as well as Storage -&gt; Instances to see the results of inventory synchronization.

![](../../.gitbook/assets/storage-providers-infrastructure-pools.jpg)

## You can also perform the same action thanks to the CLI interface: [CLI Reference](../cli-reference.md#storage-providers-management)

