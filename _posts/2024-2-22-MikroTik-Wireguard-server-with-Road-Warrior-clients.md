---
layout: post
title:  "MikroTik Wireguard server with Road Warrior clients"
categories: netdevops
tags: Network Engineer
date: 2024-2-22
---

This document is a tutorial on how to set up wireguard VPN on MikroTik for road warrior clients like iOS devices. RouterOS v7.x is needed.

Wireguard is like a series of point to point tunnels, but the same IP can be used on the side of the Wireguard system itself.* In this example, we have assigned a dedicated Wireguard subnet 192.168.66.0/24, separate from our main internal network on the Mikrotik. The Wireguard server router has the IP 192.168.66.1/24, and the Wireguard clients are 192.168.66.2, 192.168.66.3, etc. You end up with the following point to point tunnels formed:

192.168.66.1 (the Wireguard server router itself) <-----------> Wireguard client on 192.168.66.2
192.168.66.1 (the Wireguard server router itself) <-----------> Wireguard client on 192.168.66.3
etc.

These Wireguard client IPs are assigned statically, and typically use private IPs that are completely unrelated to their public IPs that these clients may actually be on. There is currently no dynamic means (ex. DHCP) for handing out IPs to Wireguard clients, which might make it unsuitable for very large RoadWarrior setups as manual configuration is needed for each user.

** Note: Technically, the Wireguard server router does not need an IP on this subnet, but if you do not give it one, you need to create static routes for your clients to be accessible. These routes are unnecessary if the Wireguard server router has an IP on this subnet as a "dynamic connected" route will exist, auto-created by the MikroTik, and this strategy will be easier for most users.*

MikroTik Wireguard server config:

Code: [Select all](https://forum.mikrotik.com/viewtopic.php?p=899406#)

```
# a private and public key will be automatically generated when adding the wireguard interface
/interface wireguard
add listen-port=13231 mtu=1420 name=wireguard1
/interface wireguard peers
# the first client added here is ipv4 only
add allowed-address=192.168.66.2/32 interface=wireguard1 public-key="replace-with-public-key-of-first-client"
# the second client is dual stack - public IPv6 should be used - replace 2001:db8:cafe:beef: with one of your /64 prefixes.
add allowed-address=192.168.66.3/32,2001:db8:cafe:beef::3/128 interface=wireguard1 public-key="replace-with-public-key-of-second-client-dual-stack"
/ip address
add address=192.168.66.1/24 interface=wireguard1 network=192.168.66.0
/ipv6 address
add address=2001:db8:cafe:beef::1/64 interface=wireguard1
```

Example iOS wireguard client config **(acts as "second client" above)**:

Code: [Select all](https://forum.mikrotik.com/viewtopic.php?p=899406#)

```
Interface: (whatever name you want to specify)
Public key: the client should automatically generate this - add this to the server above replacing "replace-with-public-key-of-second-client-dual-stack"
Addresses: 192.168.66.3/24,2001:db8:cafe:beef::3/64          (note these are different subnet masks than in the server config)
DNS servers: as desired - if you want to use the wireguard server for dns, specify 192.168.66.1

Peer:
Public key - get the public key from the wireguard interface on the mikrotik and place here
Endpoint - mydyndns.whatever:13231
Allowed IPs: 0.0.0.0/0, ::/0
```

This config will result in the client sending all traffic through the MikroTik Wireguard server. If you do not want all traffic sent through (i.e. split include), limit the peer's "Allowed IPs" to whatever subnets it should access through the tunnel rather than 0.0.0.0/0 and ::/0

You also need to create an input chain firewall rule to allow UDP traffic to destination port 13231 in order for the Wireguard tunnel to be established in the first place.

Code: [Select all](https://forum.mikrotik.com/viewtopic.php?p=899406#)

```
/ip firewall filter add action=accept chain=input comment="Allow Wireguard" dst-port=13231 protocol=udp
```

The traffic you send when connected to Wireguard will come from your Wireguard client IP, 192.168.66.2 or 192.168.66.3 in my example. As a result, you have to make sure that your MikroTik firewall is allowing this traffic, that it is being NATted etc. If your config is based on the MikroTik default configuration, one way you can do this is by adding the Wireguard interface itself to your LAN interface list, which should take care of both allowing the traffic through and NATing it.
