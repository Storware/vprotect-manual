# Nodes

To list or register node use `vprotect node` sub-command:

```text
[root@localhost vprotect]# vprotect node
Incorrect syntax: Missing required option: [-r Register node, -e Register existing node, -l List nodes]

usage: node -e <NODE_NAME> <ADMIN_LOGIN> <API_URL> | -l | -r <NODE_NAME> <ADMIN_LOGIN> <API_URL>
Node management
 -e,--register-existing <NODE_NAME> <ADMIN_LOGIN> <API_URL>   Register existing node
 -l,--list                                                    List nodes
 -r,--register <NODE_NAME> <ADMIN_LOGIN> <API_URL>            Register node
```

## Examples

* To list all nodes registerd

  ```text
  vprotect node -l
  ```

* Example node registration

  ```text
  vprotect node -r node1 admin https://localhost:8181/api
  ```

* You may need to re-register node if vProtect Server adress changes

  ```text
  vprotect node -e node1 admin https://localhost:8181/api
  ```

