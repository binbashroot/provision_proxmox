Provision Proxmox VMs
=========

A project to deploy Red Hat subscribed virtual machines to a desired proxmox host via Ansible  
#### **How it works:**
A cloud-init ready base vm with NO OS using a 2g ide "boot" disk is created.  
Import a KVM cloud image as a new disk to the vm.  
Set newly imported disk as the vm boot device.  
Remove and delete the 2G ide "boot" disk.
Set KVM cloud disk as the "boot" disk.  
Start vm  
Attach a Red Hat subscription  
Install Glibc Language packages for RHEL8 and higher  
Patch vm and reboot  
Enable the AAP2 repo for any VM tagged with "controller"  
Install ansible-core for any VM tagged with "controller"  
Change the "{{ cloud_init_user }}" password to a random UNKNOWN password   
Print a reminder to add the hosts to DNS  

---
Quick Start Instructions
------------
```
#On Ansible Host
$ ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_cloud_init
$ ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_proxmox_admin
$ ssh-copy-id -i .ssh/id_proxmox_admin.pub YOURADMINUSER@proxmox.example.com

#Connect to Proxmox Server
$ ssh -i ~/.ssh/id_proxmox_admin YOURADMINUSER@proxmox.example.com

#On Proxmox Server
# pvesm add dir qcow_images --path /opt/qcow_images
# exit

Download RHEL QCOW images from access.redhat.com and upload them to your Proxmox server.
Be sure the images are resided in the /opt/qcow_images/images dir
Take Note of the file names you downloaded

On Ansible Host
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
Edit the inventory so it has your desired settings


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

##### PROXMOX REQUIREMENTS
- Working proxmox environment
- A defined directory on proxmox server for qcow images 
- A valid proxmox user/pass with admin rights via api access from Ansible controller
- A valid proxmox user/pass with admin rights via ssh access from Ansible controller

##### ANSIBLE REQUIREMENTS
- The community.general collection is installed
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
    ansible-playbook -i inventory/provision.yml site.yml
```
### To unsubscribe and deprovision virtual machines
```
    ansible-playbook -i inventory/provision.yml site.yml --tags never -e clean=true
```
### To deprovision virtual machines 
```
    ansible-playbook site.yml --tags never 
```
### After provisioning, you can begin using the dynamic inventory [file](inventory/dynamic_proxmox_inv.yml).
```
    ansible-playbook -i inventory/dynamic.yml my_other_playbook.yml 
```
### You can also use the static inventory.yml for your inventory.
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

