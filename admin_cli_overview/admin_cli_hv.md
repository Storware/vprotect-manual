# Hypervisors

Hypervisor management module is used to add, remove hypervisors and invoke indexing task. Indexing tasks gathers information about VMs running on the hypervisor and updates their location if the VM has been moved within the pool.

To manage hypervisors in the system use `vprotect hv` sub-command.

**Note** that if you're using RHV/oVirt/Oracle VM then hypervisors will be detected automatically as a part of index task. So there is no need to define OVM hypervisors and for RHV/oVirt KVM hosts will be detected automatically.

```text
[root@localhost vprotect]# vprotect hv
Incorrect syntax: Missing required option: [-c Create hypervisor (type = ["citrix", "xen", "kvm", "proxmox"]), -d Delete hypervisor, -u Set user/password, -g Show hypervisor details, -i Index VMs on hypervisor, -l List hypervisors, -L List VMs for hypervisor, -m Modify hypervisor, -n Set node for hypervisor]

usage: hv -c <HOST> <TYPE> | -d <GUID | HOST> | -g <GUID | HOST> | -i <GUID | HOST> | -l | -L <GUID | HOST> | -m <GUID | HOST> <NEW_HOST> | -n <GUID | HOST> <NODE_GUID> | -u <GUID | HOST> <USER> <PASSWORD>

Hypervisor management
 -c,--create <HOST> <TYPE>                          Create hypervisor (type = ["citrix", "xen", "kvm", "proxmox"])
 -d,--delete <GUID | HOST>                          Delete hypervisor
 -g,--details <GUID | HOST>                         Show hypervisor details
 -i,--index <GUID | HOST>                           Index VMs on hypervisor
 -l,--list                                          List hypervisors
 -L,--list-vms <GUID | HOST>                        List VMs for hypervisor
 -m,--modify <GUID | HOST> <NEW_HOST>               Modify hypervisor
 -n,--set-node <GUID | HOST> <NODE_GUID>            Set node for hypervisor
 -u,--credentials <GUID | HOST> <USER> <PASSWORD>   Set user/password
```

## Examples

* To list all hypervisors

  ```text
  vprotect hv -l
  ```

* To show details of the given hypervisor \(by GUID\)

  ```text
  vprotect hv -g c93140b8-a898-4aff-8eef-645587ca8289
  ```

* To index VMs on the given HV:

  ```text
  vprotect hv -i c93140b8-a898-4aff-8eef-645587ca8289
  ```

* To create a hypervisor you need to execute the following commands:
  * Create HV entry with given type:

    ```text
    vprotect hv -c 1.2.3.4 citrix
    ```

  * Use GUID returned to set credentials:

    ```text
    vprotect hv -u a757c6e8-ece0-467b-b912-dfe393d1e421 root password
    ```

  * By default current node is used for created hypervisor - you may change it with this command \(first HV GUID, then node GUID\):

    ```text
    vprotect hv -n a757c6e8-ece0-467b-b912-dfe393d1e421 e2673e8f-66fc-4e9f-aaef-20958c4c2b01
    ```

