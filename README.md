# Ansible role: sshd_config hardening
This playbook can be used for security hardening with Ansible. Tested on Ubuntu 22.04 LTS, Rocky Linux 9 and AlmaLinux 9.  

## To start the role
Create a playbook and import the role. The below example assumes that the role is located under the 'roles' directory within your ansible project.
```
---
- name: Server hardening with Ansible
  hosts: hardened_host
  become: false
  tasks:
  - name: Run role_sshd_hardening
    ansible.builtin.import_role:
      name: role_sshd_hardening
      tasks_from: main
    become: true
```

## Special thanks to:
* Jeff Geerling's [ansible-role-security](https://github.com/geerlingguy/ansible-role-security/tree/master);
* Alicia Sykes [Server Setup](https://www.aliciasykes.com/blog/my-server-setup-). Especially the 'Configure SSH' part.
