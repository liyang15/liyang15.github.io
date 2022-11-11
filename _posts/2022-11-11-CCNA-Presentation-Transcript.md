---
layout: post
title:  "CCNA Presentation Transcript"
categories: netdevops
tags: Network Engineer
date: 2022-11-11
---

# CCNA Presentation Transcript

------

1. **[CCNA](https://image2.slideserve.com/3731056/slide1-l.jpg)** Cisco Certified Network Associate

2. **[Objectives](https://image2.slideserve.com/3731056/objectives-l.jpg)** • Configure DHCP in an enterprise branch network • Configure NAT; PAT on a Cisco router • IPV6 • Configure new generation RIP (RIPng) to use IPv6

3. **[DHCP](https://image2.slideserve.com/3731056/slide3-l.jpg)**

4. **[DHCP Overview](https://image2.slideserve.com/3731056/dhcp-overview-l.jpg)** The Dynamic Host Configuration Protocol (DHCP) was designed to assign IP addresses and other important network configuration information dynamically. Because desktop clients typically make up the bulk of network nodes, DHCP is an extremely useful timesaving tool for network administrators. Some devices, such as servers, should be statically assigned.

5. **[DHCP Overview](https://image2.slideserve.com/3731056/dhcp-overview1-l.jpg)**


6. **[DHCP](https://image2.slideserve.com/3731056/slide6-l.jpg)** Manual Allocation: The administrator assigns a pre-allocated IP address to the client and DHCP only communicates the IP address to the device. Automatic Allocation: DHCP automatically assigns a static IP address permanently to a device, selecting it from a pool of available addresses. There is no lease and the address is permanently assigned to a device. Dynamic Allocation: DHCP automatically dynamically assigns, or leases, an IP address from a pool of addresses for a limited period of time chosen by the server, or until the client tells the DHCP server that it no longer needs the address.

7. **[BOOTP and DHCP](https://image2.slideserve.com/3731056/bootp-and-dhcp-l.jpg)** Both DHCP and BOOTP are client/server based and use UDP ports 67 and 68.

8. **[DHCP Operation](https://image2.slideserve.com/3731056/dhcp-operation-l.jpg)**

9. **[DHCP Operation- DHCP Discovery](https://image2.slideserve.com/3731056/dhcp-operation-dhcp-discovery-l.jpg)** 1- The DHCP client sends a directed IP broadcast with a DHCP request. 2- The server notes the blank address field as well as the hardware address of the client.

10. **[DHCP Operation- DHCP Offer](https://image2.slideserve.com/3731056/dhcp-operation-dhcp-offer-l.jpg)** 3- The DHCP server picks an IP address from the available pool for the segment, as well as the other segment and global parameters. The server adds these values to the appropriate fields of the DHCP packet. 4- Using the hardware address of the client, it sends this frame back to the client.

11. **[DHCP Features](https://image2.slideserve.com/3731056/dhcp-features-l.jpg)**

12. **[Configuring DHCP](https://image2.slideserve.com/3731056/configuring-dhcp-l.jpg)** Note: The networkstatement enables DHCP on any router interfaces belonging to that network. The router will act as a DHCP server on that interface. It is also the pool of addresses that the DHCP server will use. no service dhcp disables all DHCP server and relay functionality on the router.

13. **[Configuring DHCP](https://image2.slideserve.com/3731056/configuring-dhcp1-l.jpg)** The ip dhcp excluded-address command configures the router to exclude an individual address or range of addresses when assigning addresses to clients. Other IP configuration values such as the default gateway can be set from the DHCP configuration mode.

14. **[Verifying DHCP](https://image2.slideserve.com/3731056/verifying-dhcp-l.jpg)**

15. **[Verifying DHCP](https://image2.slideserve.com/3731056/verifying-dhcp1-l.jpg)**

16. **[DHCP Client](https://image2.slideserve.com/3731056/dhcp-client-l.jpg)**

17. **[DHCP Relay](https://image2.slideserve.com/3731056/dhcp-relay-l.jpg)** DHCP clients use IP broadcasts to find the DHCP server on the segment. What happens when the server and the client are not on the same segment and are separated by a router? Routers do not forward these broadcasts. When possible, administrators should use the ip helper-address command to relay broadcast requests for these key UDP services.

18. **[Using helper addresses](https://image2.slideserve.com/3731056/using-helper-addresses-l.jpg)**

19. **[Configuring IP helper addresses](https://image2.slideserve.com/3731056/configuring-ip-helper-addresses-l.jpg)** To configure RTA e0, the interface that receives the Host A broadcasts, to relay DHCP broadcasts as a unicast to the DHCP server, use the following commands: RTA(config)#interface e0 RTA(config-if)#ip helper-address 172.24.1.9 Broadcast Unicast

20. **[Verifying and Troubleshooting DHCP](https://image2.slideserve.com/3731056/verifying-and-troubleshooting-dhcp-l.jpg)**

21. **[Verifying and Troubleshooting DHCP](https://image2.slideserve.com/3731056/verifying-and-troubleshooting-dhcp1-l.jpg)** R2# show ip dhcp conflict IP address Detection Method Detection time 192.168.1.32 Ping Feb 16 2007 12:28 PM 192.168.1.64 Gratuitous ARP Feb 23 2007 08:12 AM The server uses the ping command to detect conflicts. The client uses Address Resolution Protocol (ARP) to detect clients. If an address conflict is detected, the address is removed from the pool and not assigned until an administrator resolves the conflict.

22. **[Overview](https://image2.slideserve.com/3731056/overview-l.jpg)** NAT allows private addresses to be translated into public, routable addresses. DHCP server assigns IP dynamic addresses to devices inside the network This conserves an organizations registered IP addresses and allows the packet to be transported over public external networks, such as the Internet. A variation of NAT, called Port Address Translation (PAT), allows many internal private addresses to be translated to one or more external public address.

23. **[Benefits and Drawbacks of Using NAT](https://image2.slideserve.com/3731056/benefits-and-drawbacks-of-using-nat-l.jpg)**

24. 

25. 

26. **[How NAT Works](https://image2.slideserve.com/3731056/how-nat-works-l.jpg)** A NAT-enabled device typically operates at the border of a stub network. Devices within the internal network have private IP addresses that must be translated to public, routable addresses.

27. **[NAT Terms](https://image2.slideserve.com/3731056/nat-terms-l.jpg)** Inside local address — The IP address assigned to a host on the inside network. This address is likely to be an RFC 1918 private address. Inside global address — A legitimate IP address assigned by the RIR or service provider that represents one or more inside local IP addresses to the outside world. Outside local address — The IP address of an outside host as it appears to the inside network. Not necessarily a legitimate address, it is allocated from an address space routable on the inside. Outside global address — Reachable IP address assigned to a host on the Internet.

28. **[How NAT Works](https://image2.slideserve.com/3731056/how-nat-works1-l.jpg)**

29. **[NAT Table](https://image2.slideserve.com/3731056/nat-table-l.jpg)** The NAT table records inside to outside mappings.

30. **[Static and Dynamic NAT](https://image2.slideserve.com/3731056/static-and-dynamic-nat-l.jpg)** Static NAT is designed to allow one-to-one mapping of local and global addresses. Dynamic NAT is designed to map a private IP address to a public address. Inside

31. **[Dynamic NAT](https://image2.slideserve.com/3731056/dynamic-nat-l.jpg)**

32. **[Dynamic NAT](https://image2.slideserve.com/3731056/dynamic-nat1-l.jpg)** NAT can be dynamic or static. Dynamic NAT translates inside addresses using a pool of global addresses. Each inside local address is dynamically assigned an inside global address from an administratively defined pool of addresses. Dynamic NAT enables hosts on a private network to access the internet by translating private addresses into public addresses.

33. **[Configure Dynamic Nat](https://image2.slideserve.com/3731056/configure-dynamic-nat-l.jpg)** 1- Define a pool of global addresses to be allocated as needed. router(config)# ip nat pool pool-name start-ip end-ip netmask netmask 2- Define a standard access list to identify which hosts will be translated. router(config)# access-list number permit network mask 3- Establish dynamic source translation, identifying the access list defined in the previous step. router(config)# ip nat inside source list access-list-num pool pool-name 4- Identify interfaces as inside or outside with regard to NAT. router(config-if)# ip nat {inside|outside}

34. **[Sample Dynamic NAT Configuration](https://image2.slideserve.com/3731056/sample-dynamic-nat-configuration-l.jpg)**

35. **[Confirming NAT Operation](https://image2.slideserve.com/3731056/confirming-nat-operation-l.jpg)**

36. **[Troubleshooting NAT](https://image2.slideserve.com/3731056/troubleshooting-nat-l.jpg)** outgoing incoming

37. **[Static NAT](https://image2.slideserve.com/3731056/static-nat-l.jpg)**

38. **[Static NAT](https://image2.slideserve.com/3731056/static-nat1-l.jpg)** Permits devices with a private address to be seen on a public network. Static translations are entered directly into the configuration and are always in the translation table. Typically used for web servers.

39. **[Configure Static Nat](https://image2.slideserve.com/3731056/configure-static-nat-l.jpg)** 1- Establish static translation between inside and outside addresses. router(config)# ip nat inside source static local-ip global-ip 2- Identify interfaces as inside or outside with regard to NAT. router(config-if)# ip nat {inside|outside}

40. **[Configuring Static NAT](https://image2.slideserve.com/3731056/configuring-static-nat-l.jpg)**

41. **[NAT Overload or PAT(Port Address Translation)](https://image2.slideserve.com/3731056/nat-overload-or-pat-port-address-translation-l.jpg)**

42. **[NAT overloading (sometimes called Port Address Translation](https://image2.slideserve.com/3731056/slide42-l.jpg)** or PAT) maps multiple private IP addresses to a single public IP address or a few addresses. ISP assigns one address to your router, yet several members of your family can simultaneously surf the Internet. With NAT overloading, multiple addresses can be mapped to one or to a few addresses because each private address is also tracked by a port number. When a client opens a TCP/IP session, the NAT router assigns a port number to its source address.

43. 

44. 

45. **[Configuring PAT](https://image2.slideserve.com/3731056/configuring-pat-l.jpg)** 1- Configure a NAT pool. (Or overload an interface.) 2- Create an access list to determine which address should be translated. 3- Assign this access list to the NAT pool and set it for overload. 4- Assign inside and outside interfaces.

46. **[Overloading NAT](https://image2.slideserve.com/3731056/overloading-nat-l.jpg)** 1- Configure NAT pool Range of addresses: ip nat pool bigpool 192.168.1.33 192.168.1.57 netmask 255.255.255.224 Single address ip nat pool smallpool 192.168.1.33 192.168.1.33 netmask 255.255.255.224 2- Create a standard access list to identify which addresses should be translated access-list 24 permit 10.0.0.0 0.255.255.255 3- Assign this access list to the NAT pool and set it for overload ip nat inside source list 24 pool bigpool overload 4- Assign inside and outside interfaces router(config-if)# ip nat {inside|outside}

47. **[Configuring PAT](https://image2.slideserve.com/3731056/configuring-pat1-l.jpg)** Interface is used in place of a NAT pool.

48. **[Debug NAT translations](https://image2.slideserve.com/3731056/debug-nat-translations-l.jpg)** s= - Refers to the source IP address. a.b.c.d  w.x.y.z - Indicates that source address a.b.c.d is translated to w.x.y.z. d= - Refers to the destination IP address. [xxxx] - The value in brackets is the IP identification number. This information may be useful for debugging in that it enables correlation with other packet traces from protocol analyzers.

49. 

50. **[Dúvidas????](https://image2.slideserve.com/3731056/slide50-l.jpg)**