# Installation using Ansible playbook

You can install the complete vProtect solution using the following 2 roles, available on Ansible Galaxy:

* vProtect Server: [https://galaxy.ansible.com/xe0nic/ansible\_vprotect\_server](https://galaxy.ansible.com/xe0nic/ansible_vprotect_server)
* vProtect Node: [https://galaxy.ansible.com/xe0nic/ansible\_vprotect\_node](https://galaxy.ansible.com/xe0nic/ansible_vprotect_node)

This approach installs a server and one or more nodes on remote hosts, and generates an SSL certificate based on the server host name. The end result should be the same as an RPM-based installation without the staging setup. Configuration \(such as backup destination definition or hypervisor connectivity\) still needs to be done after installation. You can also add more nodes in the future if necessary.

## Prerequisites

You need to prepare CentOS 7/8/Stream or RHEL 7/8 minimal for vProtect \(both roles can be installed on the same or different hosts\). The Ansible control host should have Ansible installed, so that it uses Python 3.x.0

This example assumes that you have `root` access to this host and you have configured your Ansible to connect with SSH public-keys to your host. For example:

generate key:  
`ssh-keygen -f ~/.ssh/id_rsa -P ""`

and copy it to your CentOS/RHEL box:  
`ssh-copy-id -i ~/.ssh/id_rsa.pub root@YOUR_HOST`

The nodes will communicate with the vProtect Server via port 8181, so they need to be able to access it using the server's FQDN \(this needs to be resolvable\).

## Installation

This example assumes that you want to install both the vProtect Server and Node **using a single playbook** and **on the same host.** However, keep in mind that you may also install them separately by providing different target hosts and using separate playbooks like in the examples in the readme roles \(links above\).

Run these on the system from which you run Ansible playbooks:

* Install Ansible roles:

  `ansible-galaxy install xe0nic.ansible_vprotect_server    
  ansible-galaxy install xe0nic.ansible_vprotect_node`

* Create a playbook directory and change it to a working directory, i.e: `mkdir vprotect && cd vprotect`
* Create an inventory file - i.e. `hosts`:

{% code title="hosts" %}
```text
[all:vars] 
ansible_user = root

[server]
192.168.1.2

[nodes]
192.168.1.2 node_name=node1
```
{% endcode %}

* Create a playbook file - `site.yml`:

{% code title="site.yml" %}
```yaml
---

- hosts: server
  roles:
  - xe0nic.ansible_vprotect_server

- hosts: nodes
  roles:
  - xe0nic.ansible_vprotect_node
```
{% endcode %}

* Run the playbook: `ansible-playbook -i hosts site.yml`
* After installation, you should be able to log in to your vProtect Server: `https://vprotect_server_address` and your nodes should be registered and running. By default, vProtect has one admin account - `admin` with the password `vPr0tect` \(with a zero\).
* Remember to prepare your staging space as described in the [Staging space configuration](common-tasks/staging-space-configuration.md).
* Now proceed with the [Initial configuration](initial-configuration.md) instructions to configure access to the hypervisors and backup destinations.

## Variables

These two roles use just a few variables. Both plays use the `server_fqdn` variable. If not defined, the server play sets the variable `server_fqdn` to the hostname reported by the OS on which it is installed. The server play will generate an SSL certificate for this FQDN, and node play will automatically use this value if defined. You can also provide this variable manually \(either in the `hosts` file or with the extra vars switch in the `ansible-playbook` command, i.e. `-e "server_fqdn=vprotect.server.local"`

Node play needs a `node_name` for the registration process. If not provided, it will just use the hostname reported by the OS, however, keep in mind that it needs to be **unique** for each node. We recommend that you set them in the host inventory file.

Optionally, you may want to set a `db_password` for the root DB access which is set during server installation. Note, that the Server service uses its own account with an auto-generated password.

By default, vProtect uses MariaDB 10.4 for CentOS - you can control the source, distribution and version of your MariaDB with the following variables \(with their respective default values\):

```yaml
mariadb_version: "10.4"
mariadb_distro: "centos7-amd64"
mariadb_repo_url: "http://yum.mariadb.org/{{ mariadb_version }}/{{ mariadb_distro }}"
mariadb_repo_gpg_key: "https://yum.mariadb.org/RPM-GPG-KEY-MariaDB"
```

