Provision Proxmox Resources
=========

A project to deploy 
  - Virtual machines to a desired proxmox host via Ansible.  
      - Currently only limited Linux distributions are implemented at this time
  - LXC containers to a desired proxmox host via Ansible.  

----
### **Overview**
##### **Virtual Machines:**
Provisions a cloud-init ready base vm with NO OS using a 2g ide "boot" disk is created.  
Imports a KVM cloud image as a new disk to the vm.  
Sets newly imported disk as the vm boot device.  
Remove and deletes the 2G ide disk that vm was initially created with.
Set KVM cloud image disk as the "boot" disk.  
Starts vm  
Attaches a Red Hat subscription (Optional)    
To provision Proxmox containers refer to [Provision Proxmox Containers](roles/provision_proxmox_vms/README.md)

##### **LXC Containers:**
Installs an LXC container from a Proxmox template. If the template is not available, it will download the template.
To provision Proxmox containers refer to [Provision Proxmox Containers](roles/provision_proxmox_containers/README.md)

---
Quick Start Instructions 
----------------
#### **Ansible Host**
```
$ ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa_proxmox
$ ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_proxmox_admin
$ ssh-copy-id -i .ssh/id_proxmox_admin.pub user@proxmox_host 
$ git clone https://github.com/binbashroot/provision_promox.git
$ set +o history
$ export PROXMOX_PASSWORD='*******'
$ export PROXMOX_HOST='proxmox.example.com'
$ export PROXMOX_USER='jdoeo@pam'
$ export REDHAT_PASS='******'
$ export REDHAT_USER='jdoe@duck.com'
$ cd provision_proxmox/inventory
$ cp example_provision_inventory.yml provision.yml
$ cp example_containers_inventory.yml containers.yml
Edit the inventory files so they have your desired settings.  

```
#### **Proxmox Server**
```
## KVM Download Instructions
$ ssh -i ~/.ssh/id_proxmox_admin user@proxmox_host
# pvesm add dir qcow_images --path /opt/qcow_images
# cd /opt/qcow/images/images
# wget http://path/to/remote/images/${IMAGE_NAME}
See Table below for url locations

*** BEGIN NOTE ***
Be sure the files you download are listed in the the default/main.yml of the provision_proxmox_vms role.
*** END NOTE ***
# exit

```
| KVM OS | Download URL | Authenication Required|
---|---|--- 
| RHEL| access.redhat.com | Yes |
| Ubuntu| cloud-images.ubuntu.com/releases/| No |


Requirements
----
##### **ENVIRONMENT VARIABLES**
- export PROXMOX_PASSWORD='*******'
- export PROXMOX_HOST='proxmox.example.com'
- export PROXMOX_USER='jdoeo@pam'
- export REDHAT_PASS='******'
- export REDHAT_USER='jdoe@duck.com'  
##### **PYTHON MODULES**
- proxmoxer==1.3.1
- requests==2.28.1
- passlib==1.7.4
- netaddr==0.8.0
##### **PROXMOX** 
- Working proxmox environment
- A defined directory on proxmox server for qcow images 
- A valid proxmox user/pass with admin rights via api access from Ansible controller
- A valid proxmox user/pass with admin rights via ssh access from Ansible controller

##### **ANSIBLE**
- The community.general collection is installed
- The ansible.utils collection is installed
- The ansible.posix collection is installed
- Path for the private_key_file variable in the included ansible.cfg file is correct. 
- A valid virtual machine [inventory](inventory/example_provision_inventory.yml)
- A valid container [inventory](inventory/example_containers_inventory.yml)

Dependencies
------------

None

Example Playbook 
----------------
```
---
- hosts: virtual_machines
  gather_facts: false
  collections:
    community.general
    ansible.utils
  pre_tasks:
    - name: Ensure netaddr python module is installed
      ansible.builtin.pip:
        name: netaddr
        extra_args: --user 
  roles:
    - provision_proxmox_vms

- hosts: containers
  gather_facts: false
  collections:
    community.general
    ansible.utils
  roles:
    - provision_proxmox_containers
```

Document Support Links  
---------  
[Ansible](https://docs.ansible.com/)  
[Proxmox](https://pve.proxmox.com/pve-docs/)

License
-------

MIT

Contributors:
---------
Thanks to my friends/coworkers who have made this project fun.  
[mdidato](https://github.com/mdidato)  
[randyoyarzabal](https://github.com/randyoyarzabal)   

Author Information
------------------

Randy Romero  
binbashroot@duck.com

