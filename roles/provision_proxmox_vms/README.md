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

Variables
--------------
There are several variables that can be defined.  Since this is a provisioning playbook, we place the variable with the inventory because it's the best source of truth for provisioning.  Once provisioning has been completed, you can use the proxmox dynamic inventory plugin for a more accurate source of truth.    
| Type | Use Case | Example |
|---|---|---|
| Static | Provisioning |[Example](/inventory/example_provision_inventory.yml)|
| Dynamic | General use |[Example](/inventory/example_dynamic.proxmox.yml)|


Dependencies
------------

None

Example Syntax 
----------------

```
**NOTE:  Be sure to use the --limit flag as needed when provisioning or deprovisioning**

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


