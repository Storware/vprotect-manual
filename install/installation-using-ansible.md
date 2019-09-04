# Installation using Ansible

You can install complete vProtect solution using the following 2 roles, available on Ansible Galaxy:

* vProtect Server: [https://galaxy.ansible.com/xe0nic/ansible\_vprotect\_server](https://galaxy.ansible.com/xe0nic/ansible_vprotect_server)
* vProtect Node: [https://galaxy.ansible.com/xe0nic/ansible\_vprotect\_node](https://galaxy.ansible.com/xe0nic/ansible_vprotect_node)

## Prerequisites

You need to prepare CentOS 7 minimal for vProtect \(both roles can be installed on the same or different hosts\).

It is assumed that you have `root` access to this host and you have configured your Ansible to connect with SSH public-keys to your host. For example:

generate key:  
`ssh-keygen -f ~/.ssh/id_rsa -P ""`

and copy it to your CentOS box:  
`ssh-copy-id -i ~/.ssh/id_rsa.pub root@YOUR_HOST`

Nodes will communicate with vProtect Server on port 8181, so they need to be able to access it using the servers FQDN \(needs to be resolvable\). 

## Installation

This example assumes that you want to install both vProtect Server and Node **using a single playbook** and **on the same host.** However, keep in mind that you may also install them separately by providing different target hosts and using separate playbooks like in the examples in the roles readme \(links above\).

Run these on the system from which you run Ansible playbooks:

* Install Ansible roles:

  `ansible-galaxy install xe0nic.ansible_vprotect_server  
  ansible-galaxy install xe0nic.ansible_vprotect_node`

* Create playbook directory and change it working directory, i.e: `mkdir vprotect && cd vprotect`
* Create inventory file - i.e. `hosts`:

{% code-tabs %}
{% code-tabs-item title="hosts" %}
```text
[all:vars] 
ansible_user = root

[server]
192.168.1.2

[nodes]
192.168.1.2 node_name=node1
```
{% endcode-tabs-item %}
{% endcode-tabs %}

* Create playbook file - `site.yml`:

{% code-tabs %}
{% code-tabs-item title="site.yml" %}
```yaml
---

- hosts: server
  roles:
  - xe0nic.ansible_vprotect_server

- hosts: nodes
  roles:
  - xe0nic.ansible_vprotect_node
```
{% endcode-tabs-item %}
{% endcode-tabs %}

* Run playbook: `ansible-playbook -i hosts site.yml`
* After installation you should be able to login to your vProtect Server: `https://vprotect_server_address` and your nodes should be registered and running.

## Variables

These 2 roles use just a few variables. Both plays use `server_fqdn` variable. If not defined, server play sets variable `server_fqdn` to the hostname reported by the OS on which it is installed. Server play will generate SSL certificate for this FQDN, and node play automatically will use this value if defined. You can also provide this variable manually \(either in `hosts` file or with extra vars switch in `ansible-playbook` command, i.e.  `-e "server_fqdn=vprotect.server.local"` 

Node play needs `node_name` for registration process. If not provided it will just use hostname reported by OS, however keep in mind that it needs to be **unique** for each node. We recommend to set them in host inventory file.

Optionally, you may want to set `db_password` for root DB access which is set during server installation. Note, that Server service uses its own account with auto-generated password.

