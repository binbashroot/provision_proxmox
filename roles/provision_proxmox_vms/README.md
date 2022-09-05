provision_proxmox_vms
=========

A role to deploy Red Hat subscribed virtual machines to a desired proxmox host

Requirements
------------

- The community.general collection is installed
- Working proxmox environment
- A dedicated directory on proxmox server for qcow images 
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
| proxmox_api_host | string | proxmox.example.com |
| proxmox_api_pass | lookup |lookup('env','ADMIN_PROXMOX_PASSWORD') |
| proxmox_api_user | string | root@pam | 
| proxmox_node | string | promox  (will fix) |
| qcow_image_path | string | /opt/qcow_images/images |
| rhel7_image | string | rhel-server-7.9-x86_64-kvm.qcow2 |
| rhel8_image | string | rhel-8.6-x86_64-kvm.qcow2 |
| rhel9_image | string | rhel-baseos-9.0-x86_64-kvm.qcow2 |
| virtual_machine_nameservers | list | 192.168.1.100, 192.168.1.200 | 
| virtual_machines | dict | ***See defaults/main.yml*** |


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


