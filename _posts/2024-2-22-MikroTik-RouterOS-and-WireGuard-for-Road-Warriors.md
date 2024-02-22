---
layout: post
title:  "MikroTik RouterOS and WireGuard for Road Warriors"
categories: netdevops
tags: Network Engineer
date: 2024-2-22
---

## Introduction

As the world starts to open back up, I find that I am traveling more, but I still need access to my home network and Lab equipment for demos and testing. I have tried various VPNs over the years including OpenVPN and ZeroTier and IPsec. These all worked well, but required running a separate server to handle the VPN termination, and were difficult to configure and maintain. In 2020, a new player entered the ring called [WireGuard](https://www.wireguard.com/). If you don’t know what WireGuard is, here is how WireGuard describes itself:

> WireGuard is an extremely simple yet fast and modern VPN that utilizes state-of-the-art cryptography. It aims to be faster, simpler, leaner, and more useful than IPsec, while avoiding the massive headache.

As I looked at my home lab/network I decided that I wanted to run my VPN service from my primary router. My choice of home router is the [MikroTik RB3011](https://mikrotik.com/product/RB3011UiAS-RM). This router supports a few different options when it comes to VPNs, and with the release of MikroTik RouterOS 7, WireGuard has been added as a native VPN service.

This blog post will talk about how to configure MikroTik 7.5 in a “Road Warrior” configuration. A “Road Warrior” configuration allows you to connect into your remote network (in this case my home lab) from any Internet connection, and route ALL traffic through the remote (home) network. This not only gives you secure access to your home network and resources, it also ensures that my network access can not be intercepted by third parties.

## Prerequisites

- A MikroTik Router running RouterOS 7.4 or higher
- [WireGuard Client](https://www.wireguard.com/install/) for OSX (or your favorite OS Client)
- Dynamic DNS Host Name - we will configure one below, or you can use an existing DDNS host name

## Configuring RouterOS

We will configure RouterOS using the RouterOS command line. You can access the command line from either the Web UI or by connecting to to your router via SSH. I do not recommend using the WebUI to add new WireGuard clients to the system. Using the Web UI caused me two days of troubleshooting only to figure out that something in the Web UI is broken and not creating the peer entries correctly. Consider yourself warned.

Start by connecting to your RouterOS command line via ssh `user@<router IP address>`.

### Create a Dynamic DNS Hostname

You will need a way to connect to your public IP address when you are out on the road. You can use a Dynamic DNS client to configure a host name that will always be the same but resolve to your current public IP address. MikroTik routers have built in support for creating a Dynamic DNS entry that ends in *.sn.mynetname.net*, or you can use any other client. If you do not have a client, here is how you enable this feature on a MikroTik router:

```
[admin@home] > /ip cloud set ddns-enabled=yes
[admin@home] > /ip cloud print
         ddns-enabled: yes
 ddns-update-interval: none
          update-time: yes
       public-address: 1.2.3.4
  public-address-ipv6: 1a04:610:8501:2000::5
             dns-name: 530d0391a71d.sn.mynetname.net
               status: updated
```

In the example above, MikroTik has assigned *530d0391a71d.sn.mynetname.net* as a host name for your connection. Be sure you get your hostname from the output and record it for later use.

### Creating the WireGuard Interface

We will next create a WireGuard interface on the router. The interface will be called *wireguard1* and will listen on port *13231*:

```
[admin@home] > /interface wireguard
[admin@home] > add listen-port=12345 name=wireguard1
```

With the WireGuard interface created, we will assign an IP address to the interface. Note that this should be an IP on a NEW subnet, do not use an existing subnet for your WireGuard network. In the example below we will be using Network *192.168.100.0/24* and assigning the IP address *192.168.100.1* to this interface:

```
[admin@home] > /ip address
[admin@home] > add address=192.168.100.1/24 interface=wireguard1
```

With the WireGuard interface created and configured, we need to open a firewall port to allow wireguard traffic into the router:

```
[admin@home] > /ip firewall filter
[admin@home] > add action=accept chain=input comment="allow WireGuard" dst-port=12345 protocol=udp place-before=1
```

Next we will add the WireGuard interface to the **LAN** list which will allow the router to pass the traffic for all the WireGuard clients that connect going forward:

```
[admin@home] > /interface list member
[admin@home] > add interface=wireguard1 list=LAN
```

### Retrieving the WireGuard Public Key

Finally, we need to get the Router’s WireGuard **Public** key. We will need this in the next section [Configuring the OSX Client](https://xphyr.net/post/wireguard_and_routeros/#configuring-the-osx-client)

```
[admin@home] > /interface wireguard print
Flags: X - disabled; R - running
 0  R name="wireguard1" mtu=1420 listen-port=12345 private-key="cBPD6JNvbEQr73gJ7NmwepSrSPK3np381AWGvBk/QkU="
      public-key="VmGMh+cwPdb8//NOhuf1i1VIThypkMQrKAO9Y55ghG8="
```

> **NOTE:** You need to ensure that you do NOT publish the “private-key” anywhere. This would compromise your security.

At this point, you are now ready for configuring your remote clients.

## Configuring a Client

### Install the Client

We will be using the OSX Client that can be obtained from the App Store [WireGuard](https://apps.apple.com/us/app/wireguard/id1451685025?mt=12). The steps that we will use below can also be used for configuring other clients. The configuration file is used in most implementations of WireGuard.

### Configure Your Connection

Once you have installed the WireGuard client, open the configuration tool by selecting “Manage Tunnels”. We can then create our “Home Connection” tunnel, by selecting the “+” and then select “Add Empty Tunnel”.

Start by entering a **Name:**, this is the name you will use later to reference this connection. Also take note of the **Public Key:**, we will need this shortly when we configure the MikroTik device to trust this connection.

![img](https://xphyr.net/images/wgmikrotik/wg-tunnel.png)

Use the following as a template to configure your connection. Be sure you do not overwrite the *PrivateKey* that was generated by your client.

```ini
[Interface]
PrivateKey = qLZ59foscJYkFxP4L7OFjW0fWJ3oUgfBveF3pMa8/14=
Address = 192.168.100.2/24
DNS = <ip address of your MikroTik Router>

[Peer]
PublicKey = VmGMh+cwPdb8//NOhuf1i1VIThypkMQrKAO9Y55ghG8=
AllowedIPs = 0.0.0.0/0
Endpoint = 530d0391a71d.sn.mynetname.net:12345
PersistentKeepalive = 25
```

A few key entries to point out here:

- **PrivateKey** - This is generated by the client. Do not change this or share the *PrivateKey* with anyone.
- **Address** - All IPs in WireGuard are statically assigned. Pick another IP address from the network you created earlier, and make sure you don’t duplicate IPs
- **DNS** - This should point to the DNS servers *inside* your network. This ensures that you do not leak information on public networks based on your DNS resolution
- **PublicKey** - This is the *PublicKey* from [Retrieving the WireGuard Public Key](https://xphyr.net/post/wireguard_and_routeros/#retrieving-the-wireguard-public-key) section above
- **Endpoint** - This should be set to the Dynamic DNS name you got above from [Create a Dynamic DNS Hostname](https://xphyr.net/post/wireguard_and_routeros/#create-a-dynamic-dns-hostname)
- **AllowedIPs** - This is basically setting up routing on your client. In this case we want the default route (0.0.0.0/0) to go through our WireGuard interface.
- **PersistentKeepalive** - This is used to keep the tunnel up and running, and ensure that if you are behind NAT firewalls, that you do not loose your connection.

Complete the configuration by clicking **Save** to save the configuration to disk. It’s now time to configure our MikroTik router with the Public key of our client.

## Add the remote machine to the WireGuard Peer list

If you are not still connected to your MikroTik console, reconnect to your console and run the following commands to create a peer:

```
[admin@home] > /interface wireguard peers
[admin@home] > add allowed-address=192.168.100.2/32 interface=wireguard1 public-key="RoH+bXZyRY5gPxTCtsu5AFBsK2NZClExcTMqNak6hjM="
```

Be sure that you set the *allowed-address* to the IP address that you assigned in your client configuration, and you use the public-key generated by **your** client.

You can validate that your peer was added by using the *wireguard print* command to see the peer was created:

```
[admin@home] > /interface wireguard peers print
[admin@home] /interface/wireguard/peers> print
Columns: INTERFACE, PUBLIC-KEY, ENDPOINT-PORT, ALLOWED-ADDRESS
# INTERFACE    PUBLIC-KEY                                    ENDPOINT-PORT  ALLOWED-ADDRESS
0 wireguard1   RoH+bXZyRY5gPxTCtsu5AFBsK2NZClExcTMqNak6hjM=              0  192.168.100.2/32
```

At this point, you can now test your connection. From the WireGuard GUI, select the tunnel configuration and click **Activate**. You should see *Data received* and *Data sent* start to increment.

![img](https://xphyr.net/images/wgmikrotik/wg-running.png)

## Conclusion

By leveraging the WireGuard services built into your MikroTik router, you can securely connect to your home network and your home network resources. This configuration also gives you the added functionality of helping secure your Internet access when you are away from home, by leveraging your DNS servers and home internet connection for all outbound connections.
