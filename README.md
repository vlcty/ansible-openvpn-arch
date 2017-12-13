# ansible-openvpn-arch

## What it does

This ansible role creates client configs to create site-2-site VPNs with OpenVPN. So you can use this if you want to deploy an OpenVPN client to a server. The OpenVPN server has to exist.

Note #1: This role was developed for Arch. For others distros minor changes has to be done.

Note #2: This role can only deploy confis with shared keys!

## How to use it

Create a dictionary named ```openvpn_clients``` and fill it with informations. Possible values for each key are:

|  Variable | Mandatory | Explanation | Default value | Possible values |
|-----------|-----------|-------------|---------------|-----------------|
| cipher | no | Which encryption cipher to use | AES-128-CBC | AES-256-CBC and so on |
| auth | no | Which algorithm should be used | SHA256 | MD5 (If you are a dumbass), SHA128 (If you are also a dumbass), SHA512 (Hi Snowden!)
| protocol | no | Which protocol to use | udp4 | udp (auto determine IPv4 or IPv6), udp4 (UDP over IPv4 only), udp6 (UDP over IPv6 only), tcp and tcp4 and so on |
| address | yes | The address of the server. Can be a FQDN, too | - | - |
| port | no | The port of the server | 1194 | - |
| tunnel_local_address | yes | The local address of the computer inside the tunnel | - | - |
| tunnel_peer_address | yes | The peer address (from your server) inside the tunnel | - | - |
| routes | no | Routes to route through the tunnel. Contains also a dict. See example | - | - |
| key | yes | The shared key | - | - |

Example config:

```
---
openvpn_clients:
    myvpn:
        address: myrouter.example.com
        port: 443
        tunnel_local_address: 192.168.81.6
        tunnel_peer_address: 192.168.81.5
        routes:
            - { route: 192.168.3.0, netmask: 255.255.255.192 } # 192.168.3.0/27
        key: |
            -----BEGIN OpenVPN Static key V1-----
            someAlphaNumericCharacters
            -----END OpenVPN Static key V1-----

```

This will create a config called myvpn.conf and start the corresponding systemd service.
