# Ansible role: sshd_config hardening
This playbook can be used for security hardening on fresh systems with Ansible. Tested on RHEL and derivatives (Rocky Linux 9 and AlmaLinux 9). 

Please note: I tried to find the sweet spot in terms of security and usability. Hence, not all modifications seems justified in terms of security hardening. You will by no means archieve a DMZ proof server by running this playbook. I always recommend to review the code by yourself and adjust it to your needs. The main purpose of this playbook is to get users to a solid starting point.

## What is does
The role does the following:
- [ ] Updates the system.
- [ ] Installs important packages (tar, ncat, vim, yum-utils & dnf-automatic).
- [ ] Creates a lokal user.
- [ ] Enabling of SELinux (set on permissive by some hosting providers).
- [ ] Configures dnf-automatic.timer for automatic updates.
- [ ] Installs and configures firewalld as a host base firewall.
- [ ] Installs crowdsec as a security detection system.
- [ ] Hardening of SSHD daemon.
- [ ] Changed SSH port from default (= 22/tcp) to 2200/tcp.

## Variables
The role uses several variables which you can thinker with based on the level of hardening one would like to archieve. 
- **crowdsec_gpg_keys**: As the name inpliesm this variable contains the gpg keys of the crowdsec third party repository. Instructions and keys can be found on their [website](https://packagecloud.io/crowdsec/crowdsec/install#manual-rpm).
- **sshd_options**: Variables which are used to configure the /etc/ssh/sshd_config file. You are free to changes these according to your needs. There are several being used. 
- **fips140**: Only FIPS 140-2 compliant ciphers, to avoid weak encryption algorithms.
- **dnf_timer_options**: Variables which are used to configure the /etc/dnf/automatic.conf configuration file. There are important adjustments configured:
  - **upgrade_type = default**. All packages are being updates. You might want to change this to *security* if you only wish to update security packages.
  - **download_updates = yes**
  - **apply_updates = yes**
  - **reboot = when-needed**. Reboots the system when specific packages are being updates which requires a reboot such as kernel related packages. You might want to turn this of by changing this to *never* when you don't want the dnf_timer trigger reboots. You can configure the reboot time by changing the *dnf_systemd_unit_line*.
  - **dnf_systemd_unit_line**: Modifies the triggering of the systemd unit file (located at /usr/lib/systemd/system/dnf-automatic.timer). Currently it is being set to every friday at 4 AM Europe/Amsterdam timezone. This means that your system is automatically being updated once a week. Change accordingly to your needs.

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
