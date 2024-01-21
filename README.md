# Ansible role: sshd_config hardening
Harden's the /etc/ssh/sshd_config file. Use this as a template / inspiration for sshd hardening. Tested on Debian 12 (bookwork) and AlmaLinux 9 (Turquoise Kodkod). Please be carefull with Redhat related distro's (AlmaLinux, RockyLinux) with SELinux enabled. You can mitigate this by labeling the port (2200 in this case) as ssh port:

```bash
# Change the SELinux label from port 2200 to ssh_port_t
semanage port -a -t ssh_port_t -p tcp 2200
# Verify SELinux label for port 2200
semanage port -l | grep 2200
```

## Special thanks to:
* Jeff Geerling's [ansible-role-security](https://github.com/geerlingguy/ansible-role-security/tree/master);
* Alicia Sykes [Server Setup](https://www.aliciasykes.com/blog/my-server-setup). Especially the 'Configure SSH' part.
