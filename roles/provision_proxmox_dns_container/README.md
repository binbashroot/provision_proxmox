Provision_proxmox_dns_container
=========

A role to install bind into a fedora lxc container in proxmox

Requirements
------------

- NFS mount for bind forward and reverse files
  - NFS mounted to /mnt/named in lxc container which is served from the proxmox host as a bind mount
- Properly formated forward and reverse dns files
- Properly formatted named.conf file
- Host variable key with "dns" as a value

Role Variables
--------------

TBD

Dependencies
------------

TBD

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: "{{ target | default('containers') }}"
      gather_facts: false
      roles:
        - role: provision_proxmox_dns_container
          when: "'dns' in tags"


License
-------

MIT

Author Information
------------------

Randy Romero  
binbashroot@duck.com