---
layout: post
title:  "WireGuard (Site to Site VPN Example)"
categories: netdevops
tags: Network Engineer
date: 2024-2-22
---

![img](https://i0.wp.com/rickfreyconsulting.com/wp-content/uploads/2020/09/mt.png?resize=600%2C146)

# &![img](http://rickfreyconsulting.com/wp-content/uploads/2020/08/Logo_of_WireGuard.svg)

# Introduction

In this tutorial, I’ll explain how to use the WireGuard VPN as a site to site VPN across the Internet. I’ve also tried to write this tutorial in such a way that these steps will work across the Internet or can be  easily setup on your test bench. Currently, the WireGuard VPN is only   available in RouterOS 7.1beta2 and as the beta designation clearly   suggests, using this in a production environment is something that   should be weighed very carefully. That being said, in the approximately  two weeks that this version has been available, I’ve grown very excited  about this particular feature and in some (**carefully contemplated**) circumstances I would consider using this feature right now. I should   also point out that even though I have been using it everyday since it   came out, and my test environment is something that would make many   people jealous, I have not truly lived with this in a production   environment yet. WireGuard itself is well established though.

[WireGuard](https://www.wireguard.com/) is a simple, fast, and modern VPN that utilizes state-of-the-art  cryptography. Its aims  to be a better choice than IPSEC or OpenVPN. That being said, the  “buttonology” of WireGuard is unlike any other tunnel.  In fact, the  only true comparisons between WireGuard and any other  tunnel are purely conceptual. WireGuard does literally everything better than all other  tunnels before it, but there is one really profound use  case for  WireGuard… We’ve had “secure” tunnels; we’ve had complicated  tunnels;  we’ve had tunnels with many choices, but **where  WireGuard really shines is that they took some incredibly complex  mechanisms and  packaged them in a way that is effective and usable by  normal people.** By the way, in the testing I’ve done, it blows  all the other tunnels  out of the water from a performance perspective.  This tunnel is a huge  win for the entire world and in my humble opinion, we should strongly  consider abandoning all other Layer 3 tunnels and  switching over to  WireGuard as often as the situation allows. This  tunnel is really that  profound.

# The Benefits of using WireGuard

**Programming and Auditing** – Its opensource and   legitimately audit-able. This is really a big deal. I was too lazy to   count the number of lines myself, but based on what I have read,   WireGuard uses somewhere between 4,000 and 7,000 lines of code. What   this really means is that its a contender for all markets… normal   networking for small to enterprise level companies, financial   institutions, all aspects of government infrastructure, this could   potentially even be used in the nuclear industries…. Any given entity   that audits the source [code](https://www.wireguard.com/repositories/) in order to deem a product worthy of a security certification can   easily audit a code base this small. Once that is done, it literally   only takes a few minutes to compare that code to the code that is being  submitted by a vendor for some particular product or service, so   at-least for this feature, products and services can get fast tracked   thorough that process… Other things may hold up the process, but this   shouldn’t.

**Great choices have been made for you** – Years ago, I  would have argued against this, but being a consultant has changed my   mind. I have had countless conversations with customers about security   settings and practices that should be used and the truth is… most of my  customers pay me to do it the exact way I just explained to them was   wrong. To make matters worse, its pretty difficult to have a causal   conversation about things like the nuts and bolts of IPSEC. In fact,   that tends to end a lot of conversations. The normal settings for things like IPSEC are settings that we have been warned against using for over a decade, but people continue to implement them and pretend that bad   security practices somehow magically provide security. There is also an  absolutely crazy mentality that we should take something like IPSEC and  circumnavigate its security policies with another less secure tunnel,  most commonly L2TP. This is how L2TP/ IPSEC and IPIP/ IPSEC are most   commonly used and its truly psychotic. Why would anyone choice to   obliterate performance and also reduce security to make things   marginally easier?

**It solves the interface problem** – In the L2TP/ IPSEC scenario I was just complaining about, there is actually a legitimate   use case for that. IPSEC itself does not have an interface, which means  you can’t bridge that tunnel. You can’t really assign anything to that  tunnel or interact with it as you would a normal interface. On the flip  side, IPSEC does not have MTU problems which allows its to perform   better than some other tunnels. WireGuard solves this problem in a   really cleaver way and as you read this tutorial, **notice that the WireGuard interface does not get bridged.** This is because of CryptoKey Routing and I’ll say more about that   latter. You get to have the interface and you can work with the MTU   issues to some degree. WireGuard doesn’t care if the tunnel traffic is   bridge or routed… that’s true to a point anyway, because the tunnel   traffic does have to be Layer 3 traffic. WireGuard is a Layer 3 tunnel.  How you setup the tunnel will look a little strange compared to say PPP  based tunnels or IPSEC, but it still works. It looks strange becuase  its really a pseudo-interface. Its not a “fully functioning” interface;  there are somethings that it just can’t do. For example, you couldn’t   put a DHCP server on the WireGuard interface and receive an address on   the other side of the tunnel. You can’t make Layer 2 structures like   VLANs traverse the tunnel, but the Layer 3 IP traffic in the VLAN can,   so unless you need a true Layer 2 tunnel, WireGuard is currently the   best choice of tunnels. Now, if you absolutely must… you could   potentially send a Layer 2 tunnel through a WireGuard tunnel.

**CryptoKey Routing** – There isn’t another tunnel or   anything else we commonly use that uses this, so its not easy to compare to other things. However, the most complained about problem with IPSEC  was the policies. Nothing could go through the tunnel unless it matched  the policies and the policies on both sides of the tunnel had to match  exactly. If I added one new subnet to my network I had to make two   changes to the IPSEC tunnel. One policy change on either side. Adding 4  subnets meant 8 changes and 8 opportunities to make a fat finger   mistake. Pay very close attention to the “Allowed Addresses” in the   WireGuard Peer settings. This is where we interact with the CryptoKey   Routing. Its as if we could handle IPSEC policies the way OSPF handles   networks and its awesome! I realize its seems a little strange at first, but its going to solve all of those policy problems without   circumnavigating the security. And as if that wasn’t awesome enough, it  significantly boosts security while making it easier.

**Built-in Roaming** – In layman’s terms… all those   problems you had with tunnels and IP addresses changing on you, just   became a thing of the past 😉 WireGuard’s explanation of this is as   simple and perfect as anything I could come up with, so I’ll just   re-post it here. The client configuration contains an *initial*   endpoint of its single peer (the server), so that it knows where to send encrypted data before it has received encrypted data. The server   configuration doesn’t have any initial endpoints of its peers (the   clients). This is because the server discovers the endpoint of its peers by examining from where correctly authenticated data originates. If the server itself changes its own endpoint, and sends data to the clients,  the clients will discover the new server endpoint and update the   configuration just the same. Both client and server send encrypted data  to the most recent IP endpoint for which they authentically decrypted   data. **Thus, there is full IP roaming on both ends.**

# The Network Scenario

![img](https://i0.wp.com/rickfreyconsulting.com/wp-content/uploads/2020/09/WireGuard1-scaled.jpg?resize=1170%2C878)

In the network diagram above, we have two remote sites that can only  talk to each other through the Internet. Both sites have different IP   schemes. We want to build a tunnel from the Main Office (server side) to the Satellite Office (client side) and route it so that users on both   sides can reach the 10.10.10.0/24 network and the 192.168.1.0/24   network. So that you are able to build this out on your test bench, I   will also include the configuration for a router which will simulate the Internet. In the end, Host A and Host B will be able to ping each   other. If you replace either Host A or Host B with your laptop, you will able to pull up the web page of the other host. A few important notes   before we start:

1. After upgrading the RouterOS version, you have to upgrade the   router’s firmware as well. If you miss this step, nothing’s going to   work.
2. There is a minor syntax bug in Winbox for the “Endpoint” field. That field requires an IP address and a port number such as   76.59.108.9:51820. That field can not accept the port number, which is   required. It can only be entered in the CLI. That also means that if you edit the peer settings and click OK, Winbox may wipe out the port   number and inadvertently break your tunnel. If you don’t pick up on that change, it will never work.
3. Some of the steps may seem slightly counter intuitive, so look at the steps very closely.

### Host Configuration

In this example, the only purpose for Host A and Host B is to have   two devices that can pass traffic. I used two MikroTik routers simply   because it lets me use RoMON for the whole scenario, but they could be   replaced with two laptops pulling a DHCP address or something similar.   He is their configuration:

#### Host A and Host B

/ip dhcp-client add disabled=no interface=ether1 /system identity set name=Host_A /tool romon set enabled=yes

#### Internet Configuration

When you are using a router to simulate the Internet, that   configuration is really simple. Its so simple, that people look at it   and suspect something is wrong. In this case, the only thing we have to  do is add two IP addresses. I also set the system identity and turned   RoMON on.

/ip address add address=24.58.12.1/25 interface=ether3 network=24.58.12.0 add address=76.59.108.1/25 interface=ether4 network=76.59.108.0 /system identity set name=Internet /tool romon set enabled=yes

#### Main Office Gateway

I kept this configuration simple, but fairly realistic. Ether 1 is   connecting to the Internet. Ethernet ports 2-5 are bridge together and   there is a DHCP server handing out IPs. It also has a simple masquerade  rule as a normal gateway would. Its configuration is as follows:

/interface bridge add name=LAN_Bridge /interface wireguard add listen-port=51820 mtu=1420 name=WireGuard1 private-key=
 “+KMxenllgLvhYZZcqS7ZkmALgpbal+Hydi7Rlun2bm4=” /ip pool add name=dhcp_pool0 ranges=10.10.10.2-10.10.10.254 /ip dhcp-server add address-pool=dhcp_pool0 disabled=no interface=LAN_Bridge name=dhcp1 /interface bridge port add bridge=LAN_Bridge interface=ether5 add bridge=LAN_Bridge interface=ether4 add bridge=LAN_Bridge interface=ether3 add bridge=LAN_Bridge interface=ether2 /interface wireguard peers add allowed-address=172.16.1.0/24,192.168.1.0/24 endpoint=76.59.108.9:51820 
 interface=WireGuard1 public-key=”yzLttRIDuD0ZWyMVHh4UD6WJqV3hdg/p0TpFW2vQinM=” /ip address add address=10.10.10.1/24 interface=LAN_Bridge network=10.10.10.0 add address=24.58.12.17/25 interface=ether1 network=24.58.12.0 add address=172.16.1.1/24 interface=WireGuard1 network=172.16.1.0 /ip dhcp-server network add address=10.10.10.0/24 dns-server=8.8.8.8 gateway=10.10.10.1 /ip firewall nat add action=masquerade chain=srcnat out-interface=ether1 /ip route add distance=1 dst-address=0.0.0.0/0 gateway=24.58.12.1 pref-src=”” scope=30 
 target-scope=10 type=unicast add dst-address=192.168.1.0/24 gateway=172.16.1.2 type=unicast /system identity set name=”Main Ofiice GW” /tool romon set enabled=yes

#### Satellite Office Gateway

The Satellite Office Gateway is almost a mirror image of the Main   Office Gateway. The only significant difference is the IP scheme is   different. It’s configuration is as follows:

/interface bridge add name=LAN_Bridge /interface wireguard add listen-port=51820 mtu=1420 name=WireGuard1 private-key=
 “OHJyhZFzOqkp99BF2Mst3yiEfEK+A0iin2CZ1xPrqE8=” /ip pool add name=dhcp_pool0 ranges=192.168.1.2-192.168.1.254 /ip dhcp-server add address-pool=dhcp_pool0 disabled=no interface=LAN_Bridge name=dhcp1 /interface bridge port add bridge=LAN_Bridge interface=ether3 add bridge=LAN_Bridge interface=ether4 add bridge=LAN_Bridge interface=ether5 add bridge=LAN_Bridge interface=ether2 /interface wireguard peers add allowed-address=172.16.1.0/24,10.10.10.0/24 endpoint=24.58.12.17:51820 
 interface=WireGuard1 public-key=”WFWjbTBsILXwW6Rp0PEwPzR/XK2l1gXmXBeEpB5COR0=” /ip address add address=76.59.108.9/25 interface=ether1 network=76.59.108.0 add address=192.168.1.1/24 interface=LAN_Bridge network=192.168.1.0 add address=172.16.1.2/24 interface=WireGuard1 network=172.16.1.0 /ip dhcp-server network add address=192.168.1.0/24 dns-server=8.8.8.8 gateway=192.168.1.1 /ip firewall nat add action=masquerade chain=srcnat out-interface=ether1 /ip route add distance=1 dst-address=0.0.0.0/0 gateway=76.59.108.1 pref-src=”” scope=30 
 target-scope=10 type=unicast add dst-address=10.10.10.0/24 gateway=172.16.1.1 type=unicast /system identity set name=”Satellite Office GW” /tool romon set enabled=yes

### Building out the WireGuard Tunnel

Step 1: In the Main Office Router we’ll create the WireGuard   Interface. All you have to do, is give it a name. It will auto generate  the Public and Private Keys on it’s own. Each side of the tunnel will   have different public and private keys. I used UDP port 51820 because   the WireGuard Project used that particular port in their documentation. I also experimented with the MikroTik default of 13231 and other ports   and, of course, it worked just fine.

![img](https://i0.wp.com/rickfreyconsulting.com/wp-content/uploads/2020/09/WireGuard2-scaled.jpg?resize=1170%2C878)

Step 2: Repeat Step 1 for Satellite Office Gateway.

Step 3 & 4: Set the Peers on each side. The “Allowed Address”   field needs to contain all subnets that you will need to reach on the   far side. At the time of this writing, there is a bug in Winbox with the Endpoint Port. To set the Endpoint Port, you must configure it in the   CLI on both sides as shown. Don’t forget that if you make changes here,  it may remove the Endpoint port, which you can only confirm/ fix from   the CLI.

![img](https://i0.wp.com/rickfreyconsulting.com/wp-content/uploads/2020/09/WireGuard3-scaled.jpg?resize=1170%2C878)

![img](https://i0.wp.com/rickfreyconsulting.com/wp-content/uploads/2020/09/WireGuard5-scaled.jpg?resize=1170%2C878)

Step 5 & 6: Add an address to the WireGuard interface on each   router. This address is unique and special. WireGuard doesn’t handout IP addresses to the far side like SSTP or OpenVPN. **You have to manually add that IP to the WireGuard Interface.** That IP address causes a DAC (Dynamic Active Connected) route to be   populated in the main routing table and it will be the IP address that   we use latter on to create static routes. **Whatever the destination address of that DAC route is, it needs to be specified in the “Allowed Address” on the far side.** I played with multiple CIDR combinations and sumerization and it all   worked as long as the “Allowed Address” list on the far side contained   the IP address on that Interface.

![img](https://i0.wp.com/rickfreyconsulting.com/wp-content/uploads/2020/09/WireGuard4-scaled.jpg?resize=1170%2C878)

Step 7 & 8: Add a static route to each side, just as you would with any other tunnel.

![img](https://i0.wp.com/rickfreyconsulting.com/wp-content/uploads/2020/09/WireGuard6-scaled.jpg?resize=1170%2C878)

Step 9: Verify Connectivity. On the tunnel interface, you should see  TX and RX packets (You’ll almost always see packets being sent out).   Notice, if you disable and re-enable the tunnel just how quickly the   tunnel forms and can start passing packets again. Once you have   bi-directional traffic flowing, use Host A & B to send data back and forth.

![img](https://i0.wp.com/rickfreyconsulting.com/wp-content/uploads/2020/09/WireGuard7-scaled.jpg?resize=1170%2C878)

#### Notes about performance

I have had many questions about how it stacks up to other tunnels.   The short answer is it’s better and I can’t believe how much better. I   am leaving exact performance values out because there are still a few   things that I am unclear about as well. One of those things is whether   or not the encryption can be handled by an encryption co-processor or   even how multiple CPUs are coming into play. So here’s what I did. I   deliberately used an under-powered router with only 1 CPU and no   encryption co-processor. The performance should have been poor… I   designed the experiment to get poor results. That’s not what happened. I got results that were so good (over and over again) that I won’t share  with you what I saw… because I’m trying to understand how its even   possible. I double checked my worked and tried it on other platforms and I consistently got really fantastic results. This really is the best   tunnel I have ever evaluated.

Enjoy!
