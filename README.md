openvpn
=======

Installs and configures OpenVPN

Requirements
------------

This role requires Ansible 1.4 or higher.

Role Variables
--------------

| Name            | Default | Description                   |
|-----------------|---------|-------------------------------|
| openvpn_version | 2.3.8   | Version of OpenVPN to install |

Dependencies
------------

None

Example Playbook
----------------

Install OpenVPN
```
- hosts: all
  roles:
    - kbrebanov.openvpn
```

Install older version of OpenVPN
```
- hosts: all
  vars:
    openvpn_version: 2.3.6
  roles:
    - kbrebanov.openvpn
```

License
-------

BSD

Author Information
------------------

Kevin Brebanov
