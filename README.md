Provision Proxmox VMs
=========

A project to deploy virtual machines to a desired proxmox host via Ansible.  Currently only linux distributions are supported at this time.  Windows should be doable, but nothing on my roadmp at this time.
#### **How it works:**
A cloud-init ready base vm with NO OS using a 2g ide "boot" disk is created.  
Imports a KVM cloud image as a new disk to the vm.  
Sets newly imported disk as the vm boot device.  
Remove and deletes the 2G ide disk that vm was initially created with.
Set KVM cloud image disk as the "boot" disk.  
Starts vm  
Attaches a Red Hat subscription (Optional)    

**NOTE:**   
To provision Proxmox containers refer to [Provision Proxmox Containers](roles/provision_proxmox_containers/README.md)

---
Quick Start Instructions
------------
```
# On Ansible Host
----------------
$ ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa_proxmox
$ ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_proxmox_admin
$ ssh-copy-id -i .ssh/id_proxmox_admin.pub YOURADMINUSER@proxmox.example.com

#Connect to Proxmox Server
$ ssh -i ~/.ssh/id_proxmox_admin YOURADMINUSER@proxmox.example.com
# pvesm add dir qcow_images --path /opt/qcow_images
# cd /opt/qcow/images/images
# wget http://path/to/remote/images/${IMAGE_NAME}
*** NOTE ***
RHEL qcow2 images can be found at access.redhat.com
Ubuntu qcow2 images can be fount at https://cloud-images.ubuntu.com/releases/.
Be sure the downloaded images reside in the /opt/qcow_images/images dir as their final location.
Be sure file names you download are in the the default/main.yml of the provision_proxmo_vms role.
*** END NOTE ***
# exit



# On Ansible Host
----------------
$ git clone https://github.com/binbashroot/provision_promox.git
$ set +o history
$ export PROXMOX_PASSWORD='*******'
$ export PROXMOX_HOST='proxmox.example.com'
$ export PROXMOX_USER='jdoeo@pam'
$ export REDHAT_PASS='******'
$ export REDHAT_USER='jdoe@duck.com'
$ cd provision_proxmox
$ cp inventory/example_provision_inventory.yml inventory/provision.yml
$ vi inventory/provision.yml
Edit the inventory so it has your desired settings.  
Run ansible against your inventory.  See example syntax below.


```
---

Requirements
------------
##### ENVIRONMENT VARIABLE REQUIREMENTS
- export PROXMOX_PASSWORD='*******'
- export PROXMOX_HOST='proxmox.example.com'
- export PROXMOX_USER='jdoeo@pam'
- export REDHAT_PASS='******'
- export REDHAT_USER='jdoe@duck.com'  
##### PYTHON REQUIREMENTS
- proxmoxer==1.3.1
- requests==2.28.1
- passlib==1.7.4
- netaddr==0.8.0
##### PROXMOX REQUIREMENTS
- Working proxmox environment
- A defined directory on proxmox server for qcow images 
- A valid proxmox user/pass with admin rights via api access from Ansible controller
- A valid proxmox user/pass with admin rights via ssh access from Ansible controller

##### ANSIBLE REQUIREMENTS
- The community.general collection is installed
- The ansible.utils collection is installed
- The ansible.posix collection is installed
- Path for the private_key_file variable in the included ansible.cfg file is correct. 
- A valid [inventory](inventory/example_provision_inventory.yml)

##### PLAYBOOK REQUIREMENTS
- A desired cloud-init user/pass for provisioning
- A valid ssh key to be use for distributing to hosts via cloud-init
- A valid Red Hat account with access to subscriptions 

Dependencies
------------

None

Example Syntax 
----------------
### To initially provision virtual machines:

```
    To provision your whole inventory:
    ansible-playbook -i inventory/provision.yml site.yml

    To provision to a specific host in your inventory:
    ansible-playbook -i inventory/provision.yml site.yml --limit myhost

    To provision to all hosts and exclude specific host in your inventory:
    ansible-playbook -i inventory/provision.yml site.yml --limit '!myhost'
```
### To unsubscribe RHEL servers from RHSM and deprovision virtual machines
```
    ansible-playbook -i inventory/provision.yml site.yml --tags never -e clean=true
```
### To force deprovision virtual machines 
```
    ansible-playbook site.yml --tags never 
```
### After provisioning, you can begin using the dynamic inventory [file](inventory/dynamic_proxmox_inv.yml).
```
    ansible-playbook -i inventory/dynamic.proxmox.yml my_other_playbook.yml 
```
### You can also use the static inventory.yml for follow up playbooks.
```
    ansible-playbook -i inventory/provision.yml yet_another_playbook.yml 
```
---
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

