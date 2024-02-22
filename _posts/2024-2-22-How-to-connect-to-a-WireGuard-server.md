---
layout: post
title:  "How to connect to a WireGuard server"
categories: netdevops
tags: Network Engineer
date: 2024-2-22
---

```
# create Wireguard interface using keys provided by the Wireguard server
/interface/wireguard
add listen-port=13231 name=WGinterface private-key="client-private-key"

# add Wireguard peer using the endpoint address and port provided by the Wireguard server. Permit requests to any IP/ the internet by allowing 0.0.0.0/0
/interface/wireguard/peers
add allowed-address=0.0.0.0/0 endpoint-address=server.public.ip.address endpoint-port=51820 interface=WGinterface public-key="server-public-key"

# assign IP address to the Wireguard interface 
# Currently set to the interface address provided by the WG server, but I'm not sure whether this is correct. 
/ip address
add address=10.1.1.2/24 interface=WGinterface

# add a routing table to direct internet traffic via Wireguard server
/routing table 
add name=useWG fib

# add a rule to direct traffic to the routing table. If WG is down/inaccessible users can access the internet locally (i.e., action=lookup rather than  action=lookup-in-table). 
# For testing purposes src-address = all devices on my local network. I will probably constrain this to one or two specific devices once things are working. 
# After reading the guides linked above I'm unsure whether a route for return traffic (replies) is also needed, or whether this is only necessary under specific circumstances (e.g., if there is a second WG tunnel connecting to remote/ offsite users). 
/routing rule 
add src-address=192.168.1.0/24 action=lookup table=useWG

# add a route to the useWG table to force all requests to the internet (i.e., 0.0.0.0/0) to use the WG interface
/ip route
add dst-address=0.0.0.0/0 gateway=WGinterface routing-table=useWG

# The Wireguard server provided an interface address of 10.1.1.2/24. As the server is expecting traffic from 10.1.1.2/24 I think I need to sourcenat, is this correct? Does this need to be assigned to the the useWG table?
/ip firewall nat
add action=masquerade chain=srcnat out-interface=WGinterface
# jun/13/2023 07:09:03 by RouterOS 7.9.2
# software id = 1DAE-CJNN
#
# model = C53UiG+5HPaxD2HPaxD
# serial number = HE309TRDA97
/interface bridge
add admin-mac=48:A9:8A:4A:CB:53 auto-mac=no comment=defconf name=bridge
/interface wifiwave2
set [ find default-name=wifi1 ] channel.skip-dfs-channels=10min-cac \
    configuration.country=Indonesia .mode=ap .ssid=ThePromisedLAN disabled=no \
    security.authentication-types=wpa2-psk,wpa3-psk
set [ find default-name=wifi2 ] channel.skip-dfs-channels=10min-cac \
    configuration.country=Indonesia .mode=ap .ssid=ThePromisedLAN disabled=no \
    security.authentication-types=wpa2-psk,wpa3-psk
add configuration.ssid=503 disabled=no mac-address=4A:A9:8A:4A:CB:57 \
    master-interface=wifi1 name=wifi3
add configuration.ssid=503 disabled=no mac-address=4A:A9:8A:4A:CB:58 \
    master-interface=wifi2 name=wifi4
/interface list
add comment=defconf name=WAN
add comment=defconf name=LAN
/ip pool
add name=dhcp ranges=192.168.1.10-192.168.1.254
/ip dhcp-server
add address-pool=dhcp interface=bridge name=defconf
/interface bridge filter
add action=drop chain=forward in-interface=wifi3
add action=drop chain=forward out-interface=wifi3
add action=drop chain=forward in-interface=wifi4
add action=drop chain=forward out-interface=wifi4
/interface bridge port
add bridge=bridge comment=defconf interface=ether2
add bridge=bridge comment=defconf interface=ether3
add bridge=bridge comment=defconf interface=ether4
add bridge=bridge comment=defconf interface=ether5
add bridge=bridge comment=defconf interface=wifi1
add bridge=bridge comment=defconf interface=wifi2
add bridge=bridge interface=wifi3
add bridge=bridge interface=wifi4
/ip neighbor discovery-settings
set discover-interface-list=LAN
/interface list member
add comment=defconf interface=bridge list=LAN
add comment=defconf interface=ether1 list=WAN
/ip address
add address=192.168.1.1/24 comment=defconf interface=bridge network=\
    192.168.1.0
/ip dhcp-client
add comment=defconf interface=ether1
/ip dhcp-server lease
add address=192.168.1.48 client-id=1:dc:a6:32:cb:ff:cc comment=\
    "Home Controller" mac-address=DC:A6:32:CB:FF:CC server=defconf
/ip dhcp-server network
add address=192.168.1.0/24 comment=defconf dns-server=192.168.1.1 gateway=\
    192.168.1.1 netmask=24
/ip dns
set allow-remote-requests=yes
/ip dns static
add address=192.168.1.1 comment=defconf name=router.lan
add address=192.168.1.1 comment=defconf name=router.home
add address=192.168.1.48 name=homebridge.home
add address=192.168.1.145 name=synology.home
/ip firewall filter
add action=accept chain=input comment=\
    "defconf: accept established,related,untracked" connection-state=\
    established,related,untracked
add action=drop chain=input comment="defconf: drop invalid" connection-state=\
    invalid
add action=accept chain=input comment="defconf: accept ICMP" protocol=icmp
add action=accept chain=input comment=\
    "defconf: accept to local loopback (for CAPsMAN)" dst-address=127.0.0.1
add action=drop chain=input comment="defconf: drop all not coming from LAN" \
    in-interface-list=!LAN
add action=accept chain=forward comment="defconf: accept in ipsec policy" \
    ipsec-policy=in,ipsec
add action=accept chain=forward comment="defconf: accept out ipsec policy" \
    ipsec-policy=out,ipsec
add action=fasttrack-connection chain=forward comment="defconf: fasttrack" \
    connection-state=established,related hw-offload=yes
add action=accept chain=forward comment=\
    "defconf: accept established,related, untracked" connection-state=\
    established,related,untracked
add action=drop chain=forward comment="defconf: drop invalid" \
    connection-state=invalid
add action=drop chain=forward comment=\
    "defconf: drop all from WAN not DSTNATed" connection-nat-state=!dstnat \
    connection-state=new in-interface-list=WAN
/ip firewall nat
add action=masquerade chain=srcnat comment="defconf: masquerade" \
    ipsec-policy=out,none out-interface-list=WAN
/ipv6 firewall address-list
add address=::/128 comment="defconf: unspecified address" list=bad_ipv6
add address=::1/128 comment="defconf: lo" list=bad_ipv6
add address=fec0::/10 comment="defconf: site-local" list=bad_ipv6
add address=::ffff:0.0.0.0/96 comment="defconf: ipv4-mapped" list=bad_ipv6
add address=::/96 comment="defconf: ipv4 compat" list=bad_ipv6
add address=100::/64 comment="defconf: discard only " list=bad_ipv6
add address=2001:db8::/32 comment="defconf: documentation" list=bad_ipv6
add address=2001:10::/28 comment="defconf: ORCHID" list=bad_ipv6
add address=3ffe::/16 comment="defconf: 6bone" list=bad_ipv6
/ipv6 firewall filter
add action=accept chain=input comment=\
    "defconf: accept established,related,untracked" connection-state=\
    established,related,untracked
add action=drop chain=input comment="defconf: drop invalid" connection-state=\
    invalid
add action=accept chain=input comment="defconf: accept ICMPv6" protocol=\
    icmpv6
add action=accept chain=input comment="defconf: accept UDP traceroute" port=\
    33434-33534 protocol=udp
add action=accept chain=input comment=\
    "defconf: accept DHCPv6-Client prefix delegation." dst-port=546 protocol=\
    udp src-address=fe80::/10
add action=accept chain=input comment="defconf: accept IKE" dst-port=500,4500 \
    protocol=udp
add action=accept chain=input comment="defconf: accept ipsec AH" protocol=\
    ipsec-ah
add action=accept chain=input comment="defconf: accept ipsec ESP" protocol=\
    ipsec-esp
add action=accept chain=input comment=\
    "defconf: accept all that matches ipsec policy" ipsec-policy=in,ipsec
add action=drop chain=input comment=\
    "defconf: drop everything else not coming from LAN" in-interface-list=\
    !LAN
add action=accept chain=forward comment=\
    "defconf: accept established,related,untracked" connection-state=\
    established,related,untracked
add action=drop chain=forward comment="defconf: drop invalid" \
    connection-state=invalid
add action=drop chain=forward comment=\
    "defconf: drop packets with bad src ipv6" src-address-list=bad_ipv6
add action=drop chain=forward comment=\
    "defconf: drop packets with bad dst ipv6" dst-address-list=bad_ipv6
add action=drop chain=forward comment="defconf: rfc4890 drop hop-limit=1" \
    hop-limit=equal:1 protocol=icmpv6
add action=accept chain=forward comment="defconf: accept ICMPv6" protocol=\
    icmpv6
add action=accept chain=forward comment="defconf: accept HIP" protocol=139
add action=accept chain=forward comment="defconf: accept IKE" dst-port=\
    500,4500 protocol=udp
add action=accept chain=forward comment="defconf: accept ipsec AH" protocol=\
    ipsec-ah
add action=accept chain=forward comment="defconf: accept ipsec ESP" protocol=\
    ipsec-esp
add action=accept chain=forward comment=\
    "defconf: accept all that matches ipsec policy" ipsec-policy=in,ipsec
add action=drop chain=forward comment=\
    "defconf: drop everything else not coming from LAN" in-interface-list=\
    !LAN
/system clock
set time-zone-name=Asia/Jakarta
/system note
set show-at-login=no
/tool mac-server
set allowed-interface-list=LAN
/tool mac-server mac-winbox
set allowed-interface-list=LAN
```

(1) Define wireguard interface. what you have above is fine. */interface/wireguard add listen-port=13231 name=WGinterface private-key="client-private-key"*

(2) Define wireguard IP address as provided by the third party provider........ */ip address add address=10.1.1.2/24 interface=WGinterface*

(3) Define PEER SETTINGS as you have with one addition: */interface/wireguard/peers add allowed-address=0.0.0.0/0 endpoint-address=server.public.ip.address  endpoint-port=51820 interface=WGinterface public-key="server-public-key" **persistent-keep-alive=30s***

(4) Allow LAN users to enter tunnel via forward chain firewall rules. *add action=accept chain=forward in-interface-list=LAN out-interface=WGinterface*

(5) Ensure a source nat rule exists for the tunnel ***add chain=srcnat action=masquerade out-interface=WGinterface\***

Now the tought part is what users do you want to go out internet. The best bet to do this (no such flipping switch capability), one  shouldnt have to enter the router to make things work as desired. As noted below use different subnets for such purposes. For example make a WIFI SSID or two, the ones people can log into if  they want to go out Wireguard. etc.. So before we can make progress you need to better define requirements  and user groups as per the below as well.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++ As an aside not wireguard related, REMOVE bridge filters, not required.  Tells me the design is not right for the requirements. Dont use bridge filters its an advanced setting NOT for newbies or even  intermediate users, as the normal firewall rules suffice for most  traffic.

what are you trying to achieve with wifi3 and wifi4 ??? If you are looking to have three different subnets then create the  subnets and use them appropriately and if more complex ( more than one  subnet to go out an ether port ) then create vlans.

# wireguard problem

```
# 2023-12-18 17:23:58 by RouterOS 7.13
/interface bridge
add comment="defconf: local Bridge" name=bridge1 port-cost-mode=short
/interface ethernet
set [ find default-name=ether1 ] comment="defconf: local NAS" \
    disable-running-check=no name=ether1-NAS-Bonding
set [ find default-name=ether2 ] comment="defconf: local WAN" \
    disable-running-check=no name=ether2-WAN
set [ find default-name=ether3 ] comment="defconf: local LAN" \
    disable-running-check=no name=ether3-LAN
set [ find default-name=ether4 ] comment="defconf: local LAN" \
    disable-running-check=no name=ether4-LAN
set [ find default-name=ether5 ] comment="defconf: local NAS" \
    disable-running-check=no name=ether5-NAS-Bonding
set [ find default-name=ether6 ] comment="defconf: local LAN for VMs" \
    disable-running-check=no name=ether6-LAN
/interface wireguard
add listen-port=18881 mtu=1420 name=wireguard1
/interface bonding
add mode=802.3ad name=bonding1 slaves=ether1-NAS-Bonding,ether5-NAS-Bonding
/interface pppoe-client
add add-default-route=yes comment="defconf: local PPPoE client" disabled=no \
    interface=ether2-WAN name=pppoe-out1 user=
/interface list
add comment="defconf: extranet list" name=WAN
add comment="defconf: intranet list" name=LAN
add comment="onuconf: ONU list" name=ONU
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip pool
add name=pool1 ranges=10.0.0.100-10.0.0.188
/ip dhcp-server
add address-pool=pool1 bootp-support=none interface=bridge1 name=server1
/port
set 0 name=serial0
set 1 name=serial1
/queue type
add cake-diffserv=diffserv4 cake-flowmode=dual-dsthost cake-memlimit=32.0MiB \
    cake-mpu=84 cake-nat=yes cake-overhead=38 cake-overhead-scheme=ethernet \
    cake-rtt=50ms kind=cake name=cake-rx
add cake-ack-filter=filter cake-diffserv=diffserv4 cake-flowmode=dual-srchost \
    cake-memlimit=32.0MiB cake-mpu=84 cake-nat=yes cake-overhead=38 \
    cake-overhead-scheme=ethernet cake-rtt=50ms kind=cake name=cake-tx
/queue tree
add bucket-size=0.05 comment="qosconf: download queue with CAKE" max-limit=1G \
    name=cake-download packet-mark=no-mark parent=bridge1 queue=cake-rx
add bucket-size=0.03 comment="qosconf: upload queue with CAKE" max-limit=80M \
    name=cake-upload packet-mark=no-mark parent=pppoe-out1 queue=cake-tx
/routing table
add disabled=no fib name=BGP
/system logging action
add disk-file-count=100 disk-file-name=syslog name=syslog target=disk
/interface bridge port
add bridge=bridge1 interface=bonding1 internal-path-cost=10 path-cost=10
add bridge=bridge1 interface=ether3-LAN internal-path-cost=10 path-cost=10
add bridge=bridge1 interface=ether4-LAN internal-path-cost=10 path-cost=10
add bridge=bridge1 interface=ether6-LAN internal-path-cost=10 path-cost=10
/interface list member
add comment="defconf: extranet member" interface=pppoe-out1 list=WAN
add comment="defconf: intranet member" interface=bridge1 list=LAN
add comment="onuconf: ONU member" interface=ether2-WAN list=ONU
add interface=wireguard1 list=LAN
/interface wireguard peers
add allowed-address=10.88.88.2/32 interface=wireguard1 private-key=\
    "AB1K8lUicKfZBSxcpFF4ESK=" public-key=\
    "D5dOPmdxIO+mjo/r4AEA="
/ip address
add address=10.0.0.254/24 comment="defconf: local LAN IPv4 address" \
    interface=bridge1 network=10.0.0.0
add address=10.88.88.1/24 comment="defconf: local WireGuard IPv4 address" \
    interface=wireguard1 network=10.88.88.0
add address=192.168.1.2/24 comment="onuconf: link IPv4 address for ONU" \
    interface=ether2-WAN network=192.168.1.0
/ip dhcp-server lease
/ip dhcp-server network
add address=10.0.0.0/24 dns-server=10.0.0.38 gateway=10.0.0.254 netmask=24
/ip dns
set allow-remote-requests=yes cache-max-ttl=6h max-concurrent-queries=150 \
    max-udp-packet-size=8192 servers=10.0.0.38,2402:4e00::,2400:3200::1
/ip firewall address-list
add address=10.0.0.58 comment="lanconf: local proxy ipv4 (PC)" list=\
    local_proxy_ipv4
add address=10.0.0.8 comment="lanconf: local proxy ipv4 (NAS)" list=\
    local_proxy_ipv4
add address=10.0.0.68 comment="lanconf: local proxy ipv4 (Liu Iphone)" list=\
    local_proxy_ipv4
add address=10.0.0.78 comment="lanconf: local proxy ipv4 (MAC)" list=\
    local_proxy_ipv4
add address=192.168.1.1 comment="onuconf: local ONU address" list=\
    local_onu_ipv4
add address=10.0.0.0/24 comment="lanconf: local LAN address" list=\
    local_lan_ipv4
add address=10.0.0.38 comment="lanconf: local DNS server" list=local_dns_ipv4
add address=10.0.0.48 comment="lanconf: local Clash ipv4" list=\
    local_clash_ipv4
add address=h.kevinleo.xyz comment=local_service_domain list=\
    local_service_domain
add address=0.0.0.0/8 comment="defconf: RFC6890 - this network" list=\
    no_forward_ipv4
add address=169.254.0.0/16 comment="defconf: RFC6890 - link local" list=\
    no_forward_ipv4
add address=224.0.0.0/4 comment="defconf: RFC5771 - multicast" list=\
    no_forward_ipv4
add address=255.255.255.255 comment="defconf: RFC6890 - limited broadcast" \
    list=no_forward_ipv4
add address=10.88.88.0/24 comment="lanconf: local proxy ipv4 (WireGuard)" \
    list=local_proxy_ipv4
add address=10.88.88.0/24 comment="lanconf: local WireGuard ipv4" list=\
    local_wireguard_ipv4
add address=10.88.88.0/24 comment="lanconf: local LAN address" list=\
    local_lan_ipv4
/ip firewall filter
add action=accept chain=input comment="defconf:allow WireGuard" dst-port=\
    18881 protocol=udp
add action=accept chain=input comment="allow WireGuard traffic" disabled=yes \
    src-address=10.88.88.0/24
add action=accept chain=input comment="defconf: accept ICMP after RAW" \
    protocol=icmp
add action=accept chain=input comment=\
    "defconf: accept established,related,untracked" connection-state=\
    established,related,untracked
add action=drop chain=input comment="defconf: drop invalid" connection-state=\
    invalid
add action=drop chain=input comment="defconf: drop all not coming from LAN" \
    in-interface-list=!LAN
add action=accept chain=forward comment=\
    "defconf: accept established,related,untracked" connection-state=\
    established,related,untracked
add action=accept chain=forward comment="defconf: accept all coming from LAN" \
    in-interface-list=LAN
add action=drop chain=forward comment="defconf: drop invalid" \
    connection-state=invalid
add action=drop chain=forward comment="defconf: drop bad forward IPs" \
    src-address-list=no_forward_ipv4
add action=drop chain=forward comment="defconf: drop bad forward IPs" \
    dst-address-list=no_forward_ipv4
/ip firewall mangle
add action=change-mss chain=forward comment="defconf: fix IPv4 mss for WAN" \
    new-mss=clamp-to-pmtu passthrough=yes protocol=tcp tcp-flags=syn
add action=accept chain=prerouting comment="onuconf: To Clash" \
    src-address-list=local_clash_ipv4
add action=mark-routing chain=prerouting comment="onuconf: To Clash" \
    dst-address-list=!local_onu_ipv4 dst-address-type=!local dst-port=80,443 \
    new-routing-mark=BGP passthrough=yes protocol=tcp src-address-list=\
    local_proxy_ipv4
add action=accept chain=prerouting comment="onuconf: access to ONU" \
    dst-address-list=local_onu_ipv4 src-address-list=local_lan_ipv4
add action=accept chain=prerouting comment="onuconf: WireGuard access to Lan" \
    dst-address=10.0.0.0/24 src-address=10.88.88.0/24
/ip firewall nat
add action=endpoint-independent-nat chain=srcnat comment=\
    "\C8\AB\D7\B6\D0\CENAT" out-interface-list=WAN protocol=udp \
    randomise-ports=no
add action=endpoint-independent-nat chain=dstnat comment=\
    "\C8\AB\D7\B6\D0\CENAT" in-interface-list=WAN protocol=udp \
    randomise-ports=no
add action=masquerade chain=srcnat comment="onuconf: WireGuard access to Lan" \
    out-interface=wireguard1
add action=masquerade chain=srcnat comment="\B5\D8\D6\B7\CE\B1\D7\B0" \
    out-interface-list=WAN
add action=masquerade chain=srcnat comment="onuconf: access to ONU" \
    dst-address-list=local_onu_ipv4 out-interface-list=ONU src-address-list=\
    local_lan_ipv4
add action=accept chain=dstnat comment=\
    "lanconf: accept local DNS server's query (UDP)" dst-port=53 \
    in-interface-list=LAN protocol=udp src-address-list=local_dns_ipv4
add action=accept chain=dstnat comment=\
    "lanconf: accept local DNS server's query (TCP)" dst-port=53 \
    in-interface-list=LAN protocol=tcp src-address-list=local_dns_ipv4
add action=redirect chain=dstnat comment="lanconf: redirect DNS query (UDP)" \
    dst-port=53 in-interface-list=LAN protocol=udp to-ports=53
add action=redirect chain=dstnat comment="lanconf: redirect DNS query (TCP)" \
    dst-port=53 in-interface-list=LAN protocol=tcp to-ports=53
add action=dst-nat chain=dstnat comment="HTTP\B6\CB\BF\DA\D3\B3\C9\E4" \
    dst-port=2080 in-interface-list=WAN protocol=tcp to-addresses=10.0.0.8 \
    to-ports=2080
add action=dst-nat chain=dstnat comment=\
    "HTTP\B1\BE\B5\D8\B6\CB\BF\DA\BB\D8\C1\F7" dst-address-list=\
    local_service_domain dst-port=2080 in-interface-list=LAN protocol=tcp \
    to-addresses=10.0.0.8 to-ports=2080
add action=masquerade chain=srcnat comment=\
    "HTTP\B1\BE\B5\D8\B6\CB\BF\DA\BB\D8\C1\F7\CE\B1\D7\B0" dst-port=2080 \
    out-interface-list=LAN protocol=tcp src-address-list=local_lan_ipv4
/ip firewall raw
add action=accept chain=prerouting comment=\
    "defconf: enable for transparent firewall" disabled=yes
/ip route
add comment=BGP disabled=no distance=1 dst-address=0.0.0.0/0 gateway=\
    pppoe-out1 pref-src="" routing-table=BGP scope=30 suppress-hw-offload=no \
    target-scope=10
add disabled=yes distance=1 dst-address=0.0.0.0/0 gateway=wireguard1 \
    pref-src="" routing-table=BGP scope=30 suppress-hw-offload=no \
    target-scope=10
/ip service
set telnet disabled=yes
set ftp disabled=yes
set www disabled=yes
set ssh disabled=yes
set api disabled=yes
set api-ssl disabled=yes
/ip upnp
set allow-disable-external-interface=yes enabled=yes
/ip upnp interfaces
add interface=bridge1 type=internal
add interface=pppoe-out1 type=external
/ipv6 address
add advertise=no comment=\
    "\D2\AA\C4\DA\B2\BF\C9\E8\B1\B8\CA\B9\D3\C3\BF\AA\C6\F4Advertise" \
    from-pool=ipv6-public-pool-1 interface=bridge1
/ipv6 dhcp-client
add add-default-route=yes comment="defconf: local DHCPv6 client" interface=\
    pppoe-out1 pool-name=ipv6-public-pool-1 request=prefix use-peer-dns=no
/ipv6 firewall address-list
add address=fe80::/10 comment=defconf:Local-ipv6-list list=Local-ipv6-list
/ipv6 firewall filter
add action=accept chain=input comment=\
    "defconf: accept established,related,untracked" connection-state=\
    established,related,untracked
add action=drop chain=input comment="defconf: drop invaild" connection-state=\
    invalid
add action=accept chain=input comment="defconf: accept ICMPv6" protocol=\
    icmpv6
add action=accept chain=input comment=\
    "defconf:accept DHCPv6-Client prefix delegation" dst-port=546 protocol=\
    udp src-address-list=Local-ipv6-list
add action=drop chain=input comment="defconf: drop all not coming from LAN" \
    in-interface-list=!LAN
add action=accept chain=forward comment=\
    "defconf: accept established,related,untracked" connection-state=\
    established,related,untracked
add action=drop chain=forward comment="defconf: drop invaild" \
    connection-state=invalid
add action=accept chain=forward comment="defconf: accept ICMPv6" protocol=\
    icmpv6
add action=drop chain=forward comment="defconf: drop all not coming from LAN" \
    in-interface-list=!LAN
/ipv6 nd
set [ find default=yes ] disabled=yes
add dns=2402:4e00::,2400:3200::1 interface=bridge1
/routing bgp connection
add as=65530 local.role=ebgp multihop=yes name=Clash remote.address=10.0.0.48 \
    .as=65531 router-id=10.0.0.254 routing-table=BGP
/system clock
set time-zone-name=Asia/Shanghai
/system hardware
set allow-x86-64=yes
/system identity
set name=KevinRouterOS
/system logging
add action=syslog topics=critical
add action=syslog topics=error
add action=syslog topics=warning
add action=syslog topics=system
add action=syslog topics=script
add action=syslog topics=firewall
add action=syslog topics=interface
add action=syslog disabled=yes topics=wireguard
/system note
set show-at-login=no
/system ntp client
set enabled=yes
/system ntp client servers
add address=ntp.tencent.com
add address=ntp.aliyun.com
/ip firewall filter
add action=accept chain=input comment="accept established,related,untracked" connection-state=established,related,untracked
add action=drop chain=input comment="drop invalid" connection-state=invalid
add action=accept chain=input comment="accept ICMP" protocol=icmp
add action=accept chain=input comment="allow WireGuard" dst-port=\
    13231 protocol=udp
add action=accept chain=input comment="allow WireGuard traffic" disabled=yes \
    src-address=10.88.88.0/24
add action=accept chain=input src-address-list=Your-LAN-IP comment="Config Access"
add action=accept chain=input comment="Allow LAN DNS queries-UDP" \
dst-port=53 in-interface-list=LAN protocol=udp
add action=accept chain=input comment="Allow LAN DNS queries-TCP" \
dst-port=53 in-interface-list=LAN protocol=tcp
add action=drop chain=input comment="drop all else"
add action=accept chain=forward comment="accept in ipsec policy" ipsec-policy=in,ipsec
add action=accept chain=forward comment="accept out ipsec policy" ipsec-policy=out,ipsec
add action=fasttrack-connection chain=forward comment="fasttrack" connection-state=established,related
add action=accept chain=forward comment="accept established,related, untracked" connection-state=established,related,untracked
add action=drop chain=forward comment="drop invalid" connection-state=invalid
add action=accept chain=forward comment="allow internet traffic" in-interface-list=LAN out-interface-list=WAN
add action=accept chain=forward comment="Wiregurard to LAN" in-interface=wireguard1 dst-address=192.168.88.0/.24
add action=accept chain=forward comment="allow dst-nat from both WAN and LAN" connection-nat-state=dstnat
add action=drop chain=forward comment="drop all else"

/interface list member
add comment=interface=bridge list=LAN
add comment=interface=ether1 list=WAN
add interface=wireguard list=LAN
# 2023-12-21 17:25:13 by RouterOS 7.13
# software id = ER1G-WVEL
#
/ip firewall filter
add action=accept chain=input comment="Allow established,related,untracked" \
    connection-state=established,related,untracked
add action=drop chain=input comment="Drop invalid" connection-state=invalid
add action=accept chain=input comment="Allow ICMP after RAW" protocol=icmp
add action=accept chain=input comment="Allow WireGuard" dst-port=18881 \
    protocol=udp
add action=accept chain=input comment="Allow WireGuard traffic" src-address=\
    10.88.88.0/24
add action=accept chain=input comment="Allow LAN" src-address-list=\
    local_lan_ipv4
add action=accept chain=input comment="Allow LAN DNS queries-UDP" dst-port=53 \
    in-interface-list=LAN protocol=udp
add action=accept chain=input comment="Allow LAN DNS queries-TCP" dst-port=53 \
    in-interface-list=LAN protocol=tcp
add action=drop chain=input comment="Drop all else"
add action=drop chain=input comment="defconf: drop all not coming from LAN" \
    disabled=yes in-interface-list=!LAN
add action=accept chain=forward comment="Accept in ipsec policy" ipsec-policy=\
    in,ipsec
add action=accept chain=forward comment="Accept out ipsec policy" ipsec-policy=\
    out,ipsec
add action=accept chain=forward comment="Accept established,related,untracked" \
    connection-state=established,related,untracked
add action=drop chain=forward comment="Drop invalid" connection-state=invalid
add action=accept chain=forward comment="Allow internet traffic" \
    in-interface-list=LAN out-interface-list=WAN
add action=accept chain=forward comment="Allow Wiregurard to LAN" dst-address=\
    10.88.88.0/24 in-interface=wireguard1
add action=accept chain=forward comment="Allow dst-nat from both WAN and LAN" \
    connection-nat-state=dstnat
add action=accept chain=forward comment="defconf: accept all coming from LAN" \
    in-interface-list=LAN
add action=drop chain=forward comment="Drop all else"
add action=drop chain=forward comment="defconf: drop invalid" connection-state=\
    invalid disabled=yes
add action=drop chain=forward comment="defconf: drop bad forward IPs" disabled=\
    yes src-address-list=no_forward_ipv4
add action=drop chain=forward comment="defconf: drop bad forward IPs" disabled=\
    yes dst-address-list=no_forward_ipv4
# 2023-11-25 10:01:06 by RouterOS 7.12.1
# software id = XXXX-XXXX
#
# model = XXXX
# serial number = XXXX
/interface bridge
add admin-mac=XXXX auto-mac=no comment=defconf name=bridgeLan
/interface wireguard
add listen-port=13231 mtu=1420 name=wgPeer_Client
/interface list
add comment=defconf name=WAN
add comment=defconf name=LAN
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip pool
add name=default-dhcp ranges=192.168.88.10-192.168.88.254
/ip dhcp-server
add address-pool=default-dhcp interface=bridgeLan lease-time=10m name=defconf
/port
set 0 name=serial0
/interface bridge port
add bridge=bridgeLan comment=defconf interface=ether2
add bridge=bridgeLan comment=defconf interface=ether3
add bridge=bridgeLan comment=defconf interface=ether4
add bridge=bridgeLan comment=defconf interface=ether5
/ip neighbor discovery-settings
set discover-interface-list=LAN
/interface list member
add comment=defconf interface=bridgeLan list=LAN
add comment=defconf interface=ether1 list=WAN
/interface wireguard peers
add allowed-address=192.168.100.1/24,192.168.200.0/24 \
    endpoint-address=XXXX endpoint-port=51820 interface=\
    wgPeer_Client persistent-keepalive=25s preshared-key=\
    "XXXX" public-key=\
    "XXXX"
/ip address
add address=192.168.88.1/24 comment=defconf interface=bridgeLan network=\
    192.168.88.0
add address=192.168.100.2/24 interface=wgPeer_Client network=192.168.100.0
/ip dhcp-client
add comment=defconf interface=ether1
/ip dhcp-server network
add address=192.168.88.0/24 comment=defconf dns-server=192.168.88.1 gateway=\
    192.168.88.1
/ip dns
set allow-remote-requests=yes
/ip dns static
add address=192.168.88.1 comment=defconf name=router.lan
/ip firewall filter
add action=accept chain=input comment=\
    "defconf: accept established,related,untracked" connection-state=\
    established,related,untracked
add action=drop chain=input comment="defconf: drop invalid" connection-state=\
    invalid
add action=accept chain=input comment="defconf: accept ICMP" protocol=icmp
add action=accept chain=input comment=\
    "defconf: accept to local loopback (for CAPsMAN)" dst-address=127.0.0.1
add action=drop chain=input comment="defconf: drop all not coming from LAN" \
    in-interface-list=!LAN
add action=accept chain=forward comment="defconf: accept in ipsec policy" \
    ipsec-policy=in,ipsec
add action=accept chain=forward comment="defconf: accept out ipsec policy" \
    ipsec-policy=out,ipsec
add action=fasttrack-connection chain=forward comment="defconf: fasttrack" \
    connection-state=established,related hw-offload=yes
add action=accept chain=forward comment=\
    "defconf: accept established,related, untracked" connection-state=\
    established,related,untracked
add action=drop chain=forward comment="defconf: drop invalid" \
    connection-state=invalid
add action=drop chain=forward comment=\
    "defconf: drop all from WAN not DSTNATed" connection-nat-state=!dstnat \
    connection-state=new in-interface-list=WAN
/ip firewall nat
add action=src-nat chain=srcnat dst-address=192.168.100.0/24 dst-limit=\
    1,5,dst-address/1m40s limit=1,5:packet psd=21,3s,3,1 src-address=\
    192.168.88.0/24 time=0s-1d,sun,mon,tue,wed,thu,fri,sat to-addresses=\
    192.168.100.2
add action=masquerade chain=srcnat comment="defconf: masquerade" \
    ipsec-policy=out,none out-interface-list=WAN src-address=192.168.88.0/24
/ip route
add disabled=no distance=1 dst-address=0.0.0.0/0 gateway=192.168.1.1 \
    pref-src="" routing-table=main scope=30 suppress-hw-offload=no \
    target-scope=10
add disabled=no distance=1 dst-address=192.168.200.0/24 gateway=wgPeer_Client \
    pref-src=192.168.100.2 routing-table=main scope=30 suppress-hw-offload=no \
    target-scope=10
add disabled=no distance=1 dst-address=192.168.100.0/24 gateway=wgPeer_Client \
    pref-src=192.168.100.2 routing-table=main scope=30 suppress-hw-offload=no \
    target-scope=10
/ipv6 address
add address=::1 from-pool=wanv6_dhcpClient interface=ether1
/ipv6 dhcp-client
add add-default-route=yes interface=ether1 pool-name=wanv6_dhcpClient \
    request=prefix
/ipv6 firewall address-list
add address=::/128 comment="defconf: unspecified address" list=bad_ipv6
add address=::1/128 comment="defconf: lo" list=bad_ipv6
add address=fec0::/10 comment="defconf: site-local" list=bad_ipv6
add address=::ffff:0.0.0.0/96 comment="defconf: ipv4-mapped" list=bad_ipv6
add address=::/96 comment="defconf: ipv4 compat" list=bad_ipv6
add address=100::/64 comment="defconf: discard only " list=bad_ipv6
add address=2001:db8::/32 comment="defconf: documentation" list=bad_ipv6
add address=2001:10::/28 comment="defconf: ORCHID" list=bad_ipv6
add address=3ffe::/16 comment="defconf: 6bone" list=bad_ipv6
/ipv6 firewall filter
add action=accept chain=input comment=\
    "defconf: accept established,related,untracked" connection-state=\
    established,related,untracked
add action=drop chain=input comment="defconf: drop invalid" connection-state=\
    invalid
add action=accept chain=input comment="defconf: accept ICMPv6" protocol=\
    icmpv6
add action=accept chain=input comment="defconf: accept UDP traceroute" port=\
    33434-33534 protocol=udp
add action=accept chain=input comment=\
    "defconf: accept DHCPv6-Client prefix delegation." dst-port=546 protocol=\
    udp src-address=fe80::/10
add action=accept chain=input comment="defconf: accept IKE" dst-port=500,4500 \
    protocol=udp
add action=accept chain=input comment="defconf: accept ipsec AH" protocol=\
    ipsec-ah
add action=accept chain=input comment="defconf: accept ipsec ESP" protocol=\
    ipsec-esp
add action=accept chain=input comment=\
    "defconf: accept all that matches ipsec policy" ipsec-policy=in,ipsec
add action=drop chain=input comment=\
    "defconf: drop everything else not coming from LAN" in-interface-list=\
    !LAN
add action=accept chain=forward comment=\
    "defconf: accept established,related,untracked" connection-state=\
    established,related,untracked
add action=drop chain=forward comment="defconf: drop invalid" \
    connection-state=invalid
add action=drop chain=forward comment=\
    "defconf: drop packets with bad src ipv6" src-address-list=bad_ipv6
add action=drop chain=forward comment=\
    "defconf: drop packets with bad dst ipv6" dst-address-list=bad_ipv6
add action=drop chain=forward comment="defconf: rfc4890 drop hop-limit=1" \
    hop-limit=equal:1 protocol=icmpv6
add action=accept chain=forward comment="defconf: accept ICMPv6" protocol=\
    icmpv6
add action=accept chain=forward comment="defconf: accept HIP" protocol=139
add action=accept chain=forward comment="defconf: accept IKE" dst-port=\
    500,4500 protocol=udp
add action=accept chain=forward comment="defconf: accept ipsec AH" protocol=\
    ipsec-ah
add action=accept chain=forward comment="defconf: accept ipsec ESP" protocol=\
    ipsec-esp
add action=accept chain=forward comment=\
    "defconf: accept all that matches ipsec policy" ipsec-policy=in,ipsec
add action=drop chain=forward comment=\
    "defconf: drop everything else not coming from LAN" in-interface-list=\
    !LAN
/system note
set show-at-login=no
/tool mac-server
set allowed-interface-list=LAN
/tool mac-server mac-winbox
set allowed-interface-list=LAN
```
