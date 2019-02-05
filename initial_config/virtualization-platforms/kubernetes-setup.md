# Kubernetes setup

## vProtect Node preparation  

vProtect Node requires `kubectl` installed and kubeconfig with a valid token \(placed in `/opt/vprotect/.kube`\) to connect to the Kubernetes cluster. You can use the following resource to discover how to obtain the bearer token: [https://github.com/kubernetes/dashboard/wiki/Creating-sample-user](https://github.com/kubernetes/dashboard/wiki/Creating-sample-user). 

1. Paste the token into the current context in kubeconfig. 

   Example:

   ```yaml
   current-context: admin-cluster.local
   kind: Config
   preferences: {}
   users:
   - name: admin-cluster.local
     user:
       token: token here
       client-certificate-data: <REDACTED>
       client-key-data: <REDACTED>
   ```

2. Copy configs to vProtect Node. 
   * For typical Kubernetes deployment: `scp root@kubernetes.master.node:~/.kube/config /opt/vprotect/.kube/config`
   * If you use Minikube, you can copy the following files to vProtect: `sudo cp /home/user/.kube/config /opt/vprotect/.kube/config sudo cp  /home/user/.minikube/{ca.crt,client.crt,client.key} /opt/vprotect/.kube`
3. Modify the paths in `config` so they point to `/opt/vprotect/.kube` instead of `/home/user/.minikube`. Example:

   ```yaml
   - name: minikube
     user:
       client-certificate: /opt/vprotect/.kube/client.crt
       client-key: /opt/vprotect/.kube/client.key
   ```

4. Afterwards, give permissions to the `vprotect` user: `chown -R vprotect:vprotect /opt/vprotect/.kube`

Kubernetes Nodes should appear in vProtect after indexing the cluster.

**Notice:** Valid SSH credentials should be provided **for every Kubernetes node** by the user \(called _Hypervisor_ in vProtect UI\). If vProtect can't execute docker commands on Kubernetes/Openshift node, it means that it logged in as a user lacking admin privileges. Make sure you added your user to sudo/wheel group \( so it can execute commands with `sudo`\).

## Limitations

* currently we support only backups of Deployments \(persistent volumes and metadata\)
* **all deployment's pods will be paused during the backup operation** - this is required to have backup data to be consistent
* for a successful backup, every object used by the Deployment should have an `app` label assigned appropriately

