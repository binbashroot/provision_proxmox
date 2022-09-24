provision_proxmox_vms
=========

A role to deploy LXC and LXD containers to a desired proxmox host
The role will deploy a Rocky linux, Fedora, and Ubuntu server.  It will 
configure a "cloud init user" and push their ssh key.

Requirements
------------

- The community.general collection is installed
- Working proxmox environment
- A valid proxmox user/pass with admin rights with api access
- A valid proxmox user/pass with admin rights with ssh access
- A desired cloud-init user/pass for provisioning
- A valid ssh key to be use for distributing to hosts via cloud-init

Role Variables
--------------
| Variable | Type | Default |
---|---|--- 
| cloud_init_user | lookup | lookup('env','USER') | 
| cloud_init_pass | lookup | lookup('env','PROXMOX_PASSWORD') |
| cloud_init_public_key | lookup | lookup('file','~/.ssh/id_proxmox.pub') |
| domain | string | example.com |
| proxmox_api_host | string | lookup('env','PROXMOX_HOST') |
| proxmox_api_pass | lookup | lookup('env','PROXMOX_PASSWORD') |
| proxmox_api_user | string | lookup('env','PROXMOX_USER') | 
| proxmox_node | string | promox  (will fix) |
| pmsize | string | 2 |
| proxmox_storage| string | local-lvm |
| virtual_machine_nameservers | list | 192.168.1.100, 192.168.1.200 | 
| virtual_machines | dict | ***See inventory/example_provision_inventory.yml*** |

Dependencies
------------

None

Example Syntax 
----------------

```
To provision virtual machines:

    ansible-playbook containers.yml

To deprovision virtual machines:

    ansible-playbook containers.yml --tags never 

```

Example Playbook 
----------------

```
---
- hosts: localhost
  gather_facts: false
  collections:
       community.general
  pre_tasks:
     - name: Ensure netaddr python module is installed
       ansible.builtin.pip:
           name: netaddr
           extra_args: --user 
  roles:
     - provision_proxmox_vms


```

License
-------

MIT

Author Information
------------------

Randy Romero  
binbashroot@duck.com


