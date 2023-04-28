---
layout: post
title:  "WireGuard Site to Site VPN Between MikroTik RouterOS 7"
categories: netdevops
tags: Network Engineer
date: 2023-4-28
---

WireGuard is a free, open source, secure and high-speed modern VPN solution. WireGuard is extremely easy to implement but utilizes state-of-the-art cryptography. WireGuard can be used as either Client-Server VPN technology or Site to Site VPN technology.

From the RouterOS 7, MikroTik introduces WireGuard VPN as their native package. So, who are using RouterOS 7 can use WireGuard VPN and can implement both client-server and site to site VPN with **WireGuard free VPN server**.

In my previous article, I discussed [how to configure client-server free VPN server with WireGuard](https://systemzone.net/wireguard-vpn-setup-in-mikrotik-routeros7-with-windows-os/) and how to connect windows client with WireGuard VPN. In this article, I am going to show how to setup a site-to-site WireGuard VPN between two MikroTik RouterOS 7.

## **Site to Site WireGuard VPN Network Diagram**

In this article, we are going to implement a site-to-site VPN like the following image where two offices are connected over WireGuard site to site VPN service.

![Site to Site Wiregurard VPN](https://systemzone.net/wp-content/uploads/2022/09/site-to-site-wiregurard-vpn.jpg)Site to Site Wiregurard VPN

**Note:** in the above diagram, we are using private IP addresses in public interface for demo purpose. In live network, you should replace these IP Addresses with your public IP Addresses.

## **Site to Site WireGuard VPN Configuration in RouterOS 7**

According to the above network diagram, we will now configure site to site WireGuard VPN in MikroTik RouterOS. But before going to start WireGuard VPN, you should have RouterOS 7 basic configuration which includes WAN, LAN, DNS, Gateway and Masquerade setup.

If you are new in MikroTik RouterOS, feel free to study another article about [how to configure MikroTik RouterOS 7 first time](https://systemzone.net/how-to-configure-mikrotik-routeros-first-time/) and complete WAN, LAN, DNS and other Setup and then follow our WireGuard configuration steps.



If you have existing network and RouterOS 7 is running there, don’t forget to replace my demo IP information according to your existing one. You just follow my steps keeping your existing IP information.

We will now do configurations those are required for WireGuard configuration. For WireGuard configuration we need to do enabling WireGuard, Creating Peers, assigning IP address in WireGuard virtual interface and doing routing over virtual interface to communicate among LAN devices.

## **Enabling WireGuard in MikroTik RouterOS**  

WireGuard package is installed by default in MikroTik RouterOS 7. So, you will get a WireGuard menu item in Winbox by default. To enable WireGuard in R1 Router, do the following steps.

- Login to R1 Router of Office 1 with Winbox using full access user credentials.
- Click on **WireGuard** menu item from Winbox menu bar. WireGuard window will appear.
- Click on **PLUS SIGN (+)** to create a new WireGuard interface. New Interface window will appear.
- Put an interface name in **Name** input field or you can keep the default name wireguard1.
- Click **Apply** button. **Public Key** and **Private Key** will be generated as soon as you click the Apply button. The Public Key will be required when WireGuard Peer will be created in R2 Router (Office 2 Router).
- Click **OK** button.

Similarly, enable WireGuard in R2 Router of Office 2 Router and create a new WireGuard interface. Your configurations will look like the following image.

![Enabling WireGuard in RouterOS 7](https://systemzone.net/wp-content/uploads/2022/09/Enabling-WireGuard-in-RouterOS-7-1024x414.png)Enabling WireGuard in RouterOS 7

## **Assigning IP Address on WireGuard Virtual Interface**

After enabling WireGuard in RouterOS 7, a new virtual interface will be created in each Router. We will now assign IP address in each WireGuard interface so that both interfaces can communicate with each other after establishing WireGuard tunnel.

To assign IP address on WireGuard virtual interface in R1 Router, issue the following steps.

- From Winbox, go to **IP > Addresses** menu item. Address List window will appear.
- Click on **PLUS SIGN (+)** to add new address. New Address window will appear.
- Put an IP address (in this article: 10.10.10.1/30) that you to assign for WireGuard VPN tunnel in **Address** input field.
- Choose WireGuard interface (in this article: wireguard1) from **Interface** dropdown menu.
- Click **Apply** and **OK** button.

Similarly, add the second IP address on the WireGuard virtual interface of R2 Router at office 2. According to the above diagram, the second router’s IP will be 10.10.10.2/30.

## **Creating WireGuard Peers Between Two RouterOS**

After assigning IP addresses on WireGuard virtual interface, we will now configure peers in both Routers. To create peers in R1 Router of office1, issue the following steps.



- From Winbox, click on **WireGuard** menu item and then click on **Peers** tab.
- Click on **PLUS SIGN (+).** New WireGuard Peer window will appear.
- Choose WireGuard interface (wireguard1) from **Interface** dropdown menu.
- Put the Public Key that was generated at R2 Router when WireGuard was enabled, in **Public Key** input field.
- Put the Public IP address (For demo purpose, in this article: 172.26.0.2) of R1 Router in **Endpoint** input field. 
- If you don’t change the port number (default is 13231), no need to change the **Endpoint Port** but if you change, put the listen port of R1 Router in Endpoint Port input field.
- Put the IP blocks (in this article: 10.10.10.0/30 for tunnel interface and 192.168.26.0/24 LAN IP Block of R2 Router) those will be passed over WireGuard VPN Tunnel in **Allowed Address** input field. If you want to allow all IP addresses, put 0.0.0.0/0 in this field.
- In Persistent Keepalive input, put a time value in seconds (for 10 second: 00:00:10) when the tunnel will be checked and keep lived.
- Click **Apply** and **OK** button.

![Router1 Peer Configuration](https://systemzone.net/wp-content/uploads/2022/09/Router1-Peer-Configuration-1024x371.png)Peer Configuration R1 Router

Similarly, create peer in R2 Router and information accordingly. Be careful to put Public Key, Endpoint and Endpoint Port of R1 Router. Also be careful to put IP block of R2 Router’s LAN block. The configuration should be like the following image.

![Router 2 Peer Configuration](https://systemzone.net/wp-content/uploads/2022/09/Router-2-Peer-Configuration-1024x340.png)Peer Configuration in R2 Router

## **Static Routing Configuration Between RouterOS**

At the last step of site-to-site WireGuard VPN configuration, we will configure static routing between R1 and R2 Router so that R1 Router’s LAN can access R2 Router’s LAN and vice versa.



To configure static routing in R1 Router, do the following steps.

- From Winbox, go to **IP > Routes** menu item. **Route List** window will appear.
- Click PLUS SIGN (+) to add new route. **New Route** window will appear.
- In **Dst. Address** input field, put the LAN IP block (in this article: 192.168.26.0/24) of R2 Router.
- Put the IP address (10.10.10.2) assigned on WireGuard interface of R2 Router in **Gateway** input field.
- Click **Apply** and **OK** button.

![Static Routing Configuration in Router1](https://systemzone.net/wp-content/uploads/2022/09/Static-Routing-Configuration-in-Router1.png)Static Routing Configuration in R1 Router

Similarly, configure static routing in R2 Router and put the LAN IP block (in this article: 192.168.25.0/24) of R1 Router and WireGuard interface IP address (10.10.10.1) of R1 Router.

**How to configure site to site WireGuard VPN between two RouterOS** has been discussed in this article. I hope, you will now be able to configure site to site WireGuard VPN in MikroTik RouterOS. However, if you face any issue to configure site to site WireGuard VPN in MikroTik RouterOS, feel free to discuss in comment or contact me from [Contact](https://systemzone.net/contact/) page. I will try my best to stay with you.

Why not **a Cup of COFFEE** if the solution?