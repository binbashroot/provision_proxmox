provision_proxmox_vms
=========

A role to deploy virtual machines to a desired proxmox host and autosubscribe Red Hat hosts. 

Requirements
------------

- The community.general collection is installed
- Working proxmox environment
- A dedicated directory on proxmox server for qcow images with the following image names  

| Operating system | File name |   
---|---  
|rhel7| rhel-server-7.9-update-12-x86_64-kvm.qcow2 |
|rhel8| rhel-8.6-x86_64-kvm.qcow2 |
|rhel9| rhel-baseos-9.0-x86_64-kvm.qcow2 |
|ubuntu18| bionic-server-cloudimg-amd64.qcow2 |
|ubuntu20| focal-server-cloudimg-amd64.qcow2 |

- A valid proxmox user/pass with admin rights with api access
- A valid proxmox user/pass with admin rights with ssh access
- A desired cloud-init user/pass for provisioning
- A valid ssh key to be use for distributing to hosts via cloud-init
- A valid Red Hat account with access to subscriptions

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
| qcow_image_path | string | /opt/qcow_images/images |
| rhel7 | string | rhel-server-7.9-update-12-x86_64-kvm.qcow2 |
| rhel8 | string | rhel-8.6-x86_64-kvm.qcow2 |
| rhel9 | string | rhel-baseos-9.0-x86_64-kvm.qcow2 |
| ubuntu18 | string |bionic-server-cloudimg-amd64.qcow2 |
| ubuntu20 | string |focal-server-cloudimg-amd64.qcow2 |
| virtual_machine_nameservers | list | 192.168.1.100, 192.168.1.200 | 
| virtual_machines | dict | ***See inventory/example_provision_inventory.yml*** |


Dependencies
------------

None

Example Syntax 
----------------

```
To provision virtual machines:

    ansible-playbook site.yml

To deprovision virtual machines:

    ansible-playbook site.yml --tags never 

To deprovision rhsm subscribed virtual machines:

    ansible-playbook site.yml --tags never -e clean=true

```

Example Playbook 
----------------

```
---
- hosts: localhost
  gather_facts: false
  collections:
       community.general
       ansible.utils
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


