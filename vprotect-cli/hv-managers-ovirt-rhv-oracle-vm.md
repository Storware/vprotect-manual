# HV managers \(oVirt/RHV/Oracle VM\)

Hypervisor manager management module is used to add, remove hypervisor managers \(currently only oVirt and RHEV managers\) and invoke indexing task. Indexing tasks gathers information about hypervisors and VMs running in the managed environment and updates their location if the VM has been moved to the different hypervisor.

To manage HV managers in the system use `vprotect hvm` sub-command.

**Note** that if you're using RHV/oVirt/Oracle VM then hypervisors will be detected automatically as a part of index task. So there is no need to define OVM hypervisors and for RHV/oVirt KVM hosts will be detected automatically.

```text
Incorrect syntax: Missing required option: [-c Create hypervisor manager (type = ["rhev", "ovm"]), -d Delete hypervisor manager, -u Set user/password, -V List VMs managed by hypervisor manager, -i Index inventory on hypervisor manager, -l List hypervisor managers, -L List hypervisors managed by hypervisor manager, -m Modify hypervisor manager, -n Set node for hypervisor]

usage: hvm -c <URL> <TYPE> | -d <GUID | HOST> | -i <GUID | HOST> | -l | -L <GUID | HOST> | -m <GUID | HOST> <URL> | -n <GUID | HOST> <NODE_GUID | NODE_NAME> | -u <GUID | HOST> <USER> <PASSWORD> | -V <GUID |
       HOST>
Hypervisor manager management
 -c,--create <URL> <TYPE>                              Create hypervisor manager (type = ["rhev", "ovm"])
 -d,--delete <GUID | HOST>                             Delete hypervisor manager
 -i,--index <GUID | HOST>                              Index inventory on hypervisor manager
 -l,--list                                             List hypervisor managers
 -L,--list-hvs <GUID | HOST>                           List hypervisors managed by hypervisor manager
 -m,--modify <GUID | HOST> <URL>                       Modify hypervisor manager
 -n,--set-node <GUID | HOST> <NODE_GUID | NODE_NAME>   Set node for hypervisor
 -u,--credentials <GUID | HOST> <USER> <PASSWORD>      Set user/password
 -V,--list-vms <GUID | HOST>                           List VMs managed by hypervisor manager
```

## Examples

* To list all hypervisor managers

  ```text
  vprotect hvm -l
  ```

* To show details of the given HV manager \(by GUID\)

  ```text
  vprotect hvm -g 4c999c85-e223-4df3-9c40-7b0115234c8c
  ```

* To index VMs and hypervisors on the given HV manager:

  ```text
  vprotect hvm -i 4c999c85-e223-4df3-9c40-7b0115234c8c
  ```

* To create a HV manager you need to execute the following commands:
  * Create HV manger entry with given type:

    ```text
    vprotect hvm -c https://dovirt-m.lab.local/ovirt-engine/api/v3 rhev
    ```

  * Use GUID returned to set credentials:

    ```text
    vprotect hvm -u 4c999c85-e223-4df3-9c40-7b0115234c8c admin@internal password
    ```

  * By default current node is used for created HV manager - you may change it with this command \(first HV GUID, then node GUID\):

    ```text
    vprotect hvm -n 4c999c85-e223-4df3-9c40-7b0115234c8c e2673e8f-66fc-4e9f-aaef-20958c4c2b01
    ```

    Hypervisors connected to the HV manager will have node of the HV manager assigned by default. For backup export always node assigned to the HV is used.

