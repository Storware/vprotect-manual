# Deployment in Kubernetes/OpenShift environment

vProtect supports Kubernetes/OpenShift backups \(containing deployment's persistent volumes and metadata\). Node creates a small temporary pod to attach PVs from target pods and read data from there to the node \(and later moved to the backup destination\).

Keep in mind that in order to make it consistent, deployments have to be paused during backup process. Pause is done on Docker level which require access to the Kubernetes/OpenShift nodes over SSH. Especially master node is important -  vProtect Node needs to read the [kubeconfig](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/) to see your deployments.

If this mechanism is not suitable for your case - please take a look at Application backup, which allows you to backup individual applications running in your pods with native mechanisms.

![](../.gitbook/assets/kubernetes_openshift%20%281%29.png)



