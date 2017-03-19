# Ansible Roles

General-purpose reusable [Ansible][ansible] roles.

## Getting started

[Install Ansible][ansible-install], preferably in a Python [virtual environment][virtualenv]:

```
$ pip install ansible
```

Clone this repo:

```
$ cd /path/to/extra/roles
$ git clone git@github.com:mrgnr/roles.git mrgnr_roles
```

Add it to your [ansible.cfg][ansible-cfg]:

```
[defaults]
roles_path = /path/to/extra/roles/mrgnr_roles
```

Include roles in your playbooks, e.g.:

```
---

- name: my_app
  hosts: app_servers
  become_method: sudo
  become: yes
  gather_facts: True
  roles:
    - packages
    - users
    - ssh
    - fail2ban
    - unattended-upgrades
    - tor
    - ufw
    - nginx
    - letsencrypt
    - my-app
```


[ansible]: https://github.com/ansible/ansible
[ansible-install]: https://docs.ansible.com/ansible/intro_installation.html
[virtualenv]: http://docs.python-guide.org/en/latest/dev/virtualenvs/
[ansible-cfg]: https://docs.ansible.com/ansible/intro_configuration.html#roles-path
