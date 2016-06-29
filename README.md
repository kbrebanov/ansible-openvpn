openvpn
=======

[![Build Status](https://travis-ci.org/kbrebanov/ansible-openvpn.svg?branch=master)](https://travis-ci.org/kbrebanov/ansible-openvpn)

Installs and configures OpenVPN

Requirements
------------

This role requires Ansible 1.9 or higher.

To setup a static key tunnel

1. Enable **openvpn_static**
2. Enable one of **openvpn_client** or **openvpn_server**
3. Generate a secret key file and set **openvpn_secret** to the contents of the file
   *openvpn --genkey --secret static.key*
4. If this is a client, you must set **openvpn_remote_host** to the name or IP of the remote OpenVPN server

To setup a TLS server

1. Enable **openvpn_server**

To setup a TLS client

1. Enable **openvpn_client**
2. Set **openvpn_ca_certificate**, **openvpn_client_certificate** and **openvpn_client_key**
3. Set **openvpn_remote_host** to name or IP of the remote OpenVPN server

Role Variables
--------------

| Name                          | Default                                                       | Description                                                                                            |
|:------------------------------|:--------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------|
| openvpn_version               | 2.3.11                                                        | Version of OpenVPN to install                                                                          |
| openvpn_static                | false                                                         | Enable to configure a static key tunnel instead of TLS                                                 |
| openvpn_client                | false                                                         | Configure OpenVPN client                                                                               |
| openvpn_server                | false                                                         | Configure OpenVPN server                                                                               |
| openvpn_auth_enable           | false                                                         | Enable user authentication                                                                             |
| openvpn_client_endpoint       | 10.8.0.2                                                      | Client side endpoint address                                                                           |
| openvpn_server_endpoint       | 10.8.0.1                                                      | Server side endpoint address                                                                           |
| openvpn_client_ifconfig       | "{{ openvpn_client_endpoint }} {{ openvpn_server_endpoint }}" | Client ifconfig setting                                                                                |
| openvpn_server_ifconfig       | "{{ openvpn_server_endpoint }} {{ openvpn_client_endpoint }}" | Server ifconfig setting                                                                                |
| openvpn_client_pull           | false                                                         | Indicates to OpenVPN client that it should accept options pushed by the server                         |
| openvpn_dev                   | tun                                                           | VPN device                                                                                             |
| openvpn_comp_lzo              | true                                                          | Enable or disable compression                                                                          |
| openvpn_daemon                | true                                                          | Run OpenVPN as a daemon                                                                                |
| openvpn_dhcp_option_dns       | ["208.67.222.222", "208.67.220.220"]                          | DNS server addresses to push to client in TLS mode                                                     |
| openvpn_ifconfig_pool_persist | ipp.txt                                                       | Persist/unpersist ifconfig-pool data to file                                                           |
| openvpn_keepalive             | '10 60'                                                       | Set timeouts                                                                                           |
| openvpn_nobind                | true                                                          | Do not bind to local address and port                                                                  |
| openvpn_ns_cert_type          | ''                                                            | Require that peer certificate was signed with explicit nsCertType designation of "client" or "server"  |
| openvpn_persist_key           | true                                                          | Don't re-read key files across SIGUSR1 or --ping-restart                                               |
| openvpn_persist_tun           | true                                                          | Don't close and reopen TUN/TAP device or run up/down scripts across SIGUSR1 or --ping-restart restarts |
| openvpn_ping_timer_rem        | true                                                          | Run the --ping-exit / --ping-restart timer only if we have a remote address                            |
| openvpn_proto                 | udp                                                           | Protocol to use for communicating with remote host                                                     |
| openvpn_redirect_gateway      | false                                                         | Enable to redirect all client traffic thru VPN tunnel                                                  |
| openvpn_remote_host           | ''                                                            | Remote host name or IP address                                                                         |
| openvpn_remote_port           | 1194                                                          | Remote host port                                                                                       |
| openvpn_remote_proto          | "{{ openvpn_proto }}"                                         | Remote host protocol                                                                                   |
| openvpn_resolv_retry          | infinite                                                      | If hostname resolve fails for --remote, retry resolve for n seconds before failing                     |
| openvpn_secret                | ''                                                            | Secret static key                                                                                      |
| openvpn_server_subnet         | 10.8.0.0                                                      | OpenVPN TLS server subnet address                                                                      |
| openvpn_server_subnet_mask    | 255.255.255.0                                                 | OpenVPN TLS server subnet mask                                                                         |
| openvpn_status                | openvpn-status.log                                            | Write operational status to file                                                                       |
| openvpn_verb                  | 1                                                             | Set output verbosity                                                                                   |
| openvpn_ca_certificate        | ''                                                            | Specify CA certificate content for client configuration                                                |
| openvpn_client_certificate    | ''                                                            | Specify client certificate content for client configuration                                            |
| openvpn_client_key            | ''                                                            | Specify client key content for client configuration                                                    |

Dependencies
------------

- kbrebanov.easy_rsa (When using TLS)

Example Playbook
----------------

Install OpenVPN server and use static key tunnel
```yaml
- hosts: all
  vars:
    openvpn_server: true
    openvpn_secret: 'secret'
  roles:
    - kbrebanov.openvpn
```

Install older version of OpenVPN server and use static key tunnel
```yaml
- hosts: all
  vars:
    openvpn_version: 2.3.6
    openvpn_server: true
    openvpn_secret: 'secret'
  roles:
    - kbrebanov.openvpn
```

Install OpenVPN client using static key tunnel
```yaml
- hosts: all
  vars:
    openvpn_client: true
    openvpn_secret: ''
    openvpn_remote_host: 'remote.vpn.com'
  roles:
    - kbrebanov.openvpn
```

Install OpenVPN server using TLS and redirect all client traffic thru tunnel
```yaml
- hosts: all
  vars:
    openvpn_server: true
    openvpn_redirect_gateway: true
  roles:
    - kbrebanov.openvpn
```

Install OpenVPN client using TLS
```yaml
- hosts: all
  vars:
    openvpn_client: true
    openvpn_remote_host: 'remote.vpn.com'
    openvpn_ca_certificate: |
      CA cert contents
    openvpn_client_certificate: |
      client cert contents
    openvpn_client_key: |
      client key contents
  roles:
    - kbrebanov.openvpn
```

License
-------

BSD

Author Information
------------------

Kevin Brebanov
