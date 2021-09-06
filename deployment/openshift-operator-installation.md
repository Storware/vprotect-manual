# OpenShift Operator Installation

vProtect can also be installed in OpenShift environments using OperatorHub with just a few easy steps. Once installed, the operator can be used to create deployment with pods for: vProtect Server, vProtect Node, MariaDB and the operator's deployment pod. The operator also allows you to specify different storage configuration \(using a persistent volume\) or external backup providers \(only file or object based storage are supported in the operator\).

**Note:**

* vProtect Operator is intended to be used with OpenShift only - if you intend to protect other Virtualization Platforms - you may need to deploy vProtect using other methods on a VM or physical server.
* Some of the features may not be available because of container restrictions \(compared to VM deployment\).

vProtect Operator will be installed from OperatorHub available in the Openshift GUI.

![Operator Hub](../.gitbook/assets/deployment-openshift-operator_hub.jpg)

Type "vprotect" in the search field.

![Searching vprotect](../.gitbook/assets/deployment-openshift-vprotect_search.jpg)

Choose the operator without the "marketplace" label and click on the **Install** button.

![vProtect Operator](../.gitbook/assets/deployment-openshift-vprotect_operator.jpg)

The operator will be successfully installed.

![Installed vProtect Operator](../.gitbook/assets/deployment-openshift-installed_operator.jpg)

Go to the Storage -&gt; StorageClasses tab and check the available storage classes. In this case it is **ovirt-csi-sc.**

![Storage Classes](../.gitbook/assets/deployment-openshift-storage_classes.jpg)

Choose vProtect operator from list of installed operators and go to the **YAML** tab.

![vProtect Operator Yaml](../.gitbook/assets/deployment-openshift-operator_yaml.jpg)

We need to edit the fields in yaml below by entering the values from the storage classes and a valid vprotect server url.

```text
"storageClassName": " ovirt-csi-sc ",
"storageClass": "ovirt-csi-sc"
"stagingStorageClass": "ovirt-csi-sc"
"server": {
           ...
           "image": "registry.connect.redhat.com/storware/vprotect-server",
           ...
           },
           spec:
             containers:
             ...
                - name: RELATED_IMAGE_SERVER
                  value: >- registry.connect.redhat.com/storware/vprotect-server
               ...
```

Example of a valid [_yaml file_](https://storware-my.sharepoint.com/:u:/g/personal/a_spinek_storware_eu/ESap14S4R4xMvRr6HM44EjYB8eO3VWhog9WnD9gOiUESUg?e=qLYbDj)_._

After the changes, click on Save.

Go to the **Create vProtect Server - vProtect Node - DB** tab and click on the **Create vProtectDBServerNode** button.

![vProtect Server Node DB](../.gitbook/assets/deployment-openshift-vprotect_server_dbnodes.jpg)

Provide your deployment name and click on the **Create** button.

![Creating vProtect](../.gitbook/assets/deployment-openshift-create_vprotect_dbnodes.jpg)

You can check the progress on the Workloads -&gt; Pods tab. After installation, every pod should be in running status.

![Pods](../.gitbook/assets/deployment-openshift-pods.jpg)

Go to the Networking -&gt; Routes tab and click on the **Create Route** button.

![Routes](../.gitbook/assets/deployment-openshift-routes.jpg)

Provide whatever url you want vProtect to be accessible from. Choose **vprotect-server** as the **Service** and **8080 -&gt; 8080** or **8181 -&gt; 8181** as the **Target Port.**

![Route config](../.gitbook/assets/deployment-openshift-route_config.jpg)

To check the effective URL for vProtect, click on the newly created route and read the **Location** field.

![vProtect location](../.gitbook/assets/deployment-openshift-vprotect_full.jpg)

By default, the vProtect console has one admin account - `admin` with the password `vPr0tect` \(with a zero\).

