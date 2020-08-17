openvpn
========

The present role :
  - installs OpenVPN and dependancies
  - configures it as a server
  - creates client certificates and configuration file

It has been tested on :
  - Debian 9
  - Debian 10

Role variables
--------------

| Variable                           | Type    | Choices      | Default                                                                       | Comment                                                                    |
|------------------------------------|---------|--------------|-------------------------------------------------------------------------------|----------------------------------------------------------------------------|
| openvpn_user                       | string  |              | nobody                                                                        | Set the OpenVPN service user.                                              |
| openvpn_group                      | string  |              | nogroup                                                                       | Set the OpenVPN service group.                                             |
| openvpn_public_ip                  | string  |              |                                                                               | Set the OpenVPN IP on which it will be reachable.                          |
| openvpn_port                       | int     |              | 1194                                                                          | Which TCP/UDP port should OpenVPN listen on ?                              |
| openvpn_proto                      | string  |              | tcp                                                                           | TCP or UDP server?                                                         |
| openvpn_dev                        | string  |              | tun                                                                           | Set the OpenVPN type off internal network device (tun or tap).             |
| openvpn_ip_range                   | string  |              | 10.8.0.0                                                                      | Set the OpenVPN internal IP range.                                         |
| openvpn_ip_netmask                 | string  |              | 255.255.255.0                                                                 | Set the OpenVPN internal netmask.                                          |
| openvpn_compress                   | string  |              | lz4-v2                                                                        | Set the kind of compression type.                                          |
| openvpn_maxclients                 | int     |              | 10                                                                            | Set number of maximum clients allowed.                                     |
| openvpn_keepalive_ping             | int     |              | 10                                                                            | Set "keepalive" ping interval in seconds.                                  |
| openvpn_keepalive_timeout          | int     |              | 120                                                                           | Set "keepalive" timeout in seconds.                                        |
| openvpn_cipher                     | string  |              | AES-256-GCM                                                                   | Set "cipher" option for server and client.                                 |
| openvpn_auth                       | string  |              | SHA384                                                                        | Set "auth" option for authentication algoritm.                             |
| openvpn_tls_cipher                 | string  |              | TLS-ECDHE-RSA-WITH-AES-256-GCM-SHA384:TLS-ECDHE-ECDSA-WITH-AES-256-GCM-SHA384 | Set "tls-cipher' option.                                                   |
| openvpn_client_to_client           | boolean | true , false | false                                                                         | Allow clients to "see" eachother. By default client only see the server.   |
| openvpn_push_route                 | list    |              | []                                                                            | List of route to push in order to allow client access.                     |
| openvpn_verbosity                  | int     |              | 3                                                                             | Set the log verbosity.                                                     |
| openvpn_mute                       | int     |              | 20                                                                            | Set the number of consecutive messages in the same category in the log.    |
| openvpn_tls_auth                   | boolean | true , false | false                                                                         | Enable or disable tls authentication.                                      |
| openvpn_log_status                 | string  |              | /var/log/openvpn-status.log                                                   | Path of the status log file. File is truncated and rewritten evert minute. |
| openvpn_log_append                 | string  |              | /var/log/openvpn.log                                                          | Path of log file.                                                          |
| openvpn_easyrsa_req_country        | string  |              |                                                                               | Easy-RSA variable "EASYRSA_REQ_COUNTRY"                                    |
| openvpn_easyrsa_req_province       | string  |              |                                                                               | Esay-RSA variable "EASYRSA_REQ_PROVINCE"                                   |
| openvpn_easyrsa_req_city           | string  |              |                                                                               | Esay-RSA variable "EASYRSA_REQ_CITY"                                       |
| openvpn_easyrsa_req_org            | string  |              |                                                                               | Esay-RSA variable "EASYRSA_REQ_ORG"                                        |
| openvpn_easyrsa_req_email          | string  |              |                                                                               | Esay-RSA variable "EASYRSA_REQ_EMAIL"                                      |
| openvpn_easyrsa_req_ou             | string  |              |                                                                               | Esay-RSA variable "EASYRSA_REQ_OU"                                         |

Dependencies
------------

Does not depend on any other roles.

Example Playbook
----------------

    - hosts: vpn
      ignore_errors: "{{ ansible_check_mode }}" # ignore errors only in check mode !

      roles:
        - { role: brainsys.openvpn, tags: ['openvpn'] }

Example variables
-----------------

    openvpn_public_ip: a.b.c.d

    openvpn_client_to_client: false
    openvpn_proto: 'tcp'
    openvpn_ip_range: '10.8.0.0'
    openvpn_push_route:
      - ip: a.b.c.d
        netmask: 255.255.255.0
    openvpn_client:
      - name: client1
        ip: 10.8.0.11
      - name: client2
        ip: 10.8.0.12


TODO
----

  - sets up networking / firewall
  - handle delegate_to instead of lookfile when creating client configuration

License
-------

GPLv3

Author Information
------------------

Written by Ludovic Cartier <ludovic.cartier@brainsys.io>
