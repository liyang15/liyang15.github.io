---
layout: post
title:  "Configuring your Mikrotik as a WireGuard VPN Client"
categories: netdevops
tags: Network Engineer
date: 2024-2-22
---

**Follow the steps set out below to setup a VPN connection using Wireguard from a Mikrotik Client Router. ** *This guide assumes you have RouterOS version 7.6 or newer.*

1. Create a VPN user if you have not done so already from the client zone.

2. Select " Management VPN account " and note the details listed  under the " WireGuard configuration " heading which will appear similar  to the below example.

   ![57659f531007441b6bb607bc2d699d2e1ff2a6f35516cf19ddd2fc0521599a85fd89a06bbcf720d9?t=43d6f72fc10be53477af4a94a2e450ac](https://help.rackzar.com/en/download/57659f531007441b6bb607bc2d699d2e1ff2a6f35516cf19ddd2fc0521599a85fd89a06bbcf720d9?t=43d6f72fc10be53477af4a94a2e450ac)

3. Login to Mikrotik via Winbox

   Click on the menu item **WireGuard** In the window that opens, in the **WireGuard** tab, click the plus to add a new **WireGuard** interface.

   ![9e1283f6c94cfd895f66c93aa3cc7cef5ffa35dbad4aa143aaf309ae9c76ac1314e3533c7b10e0ad?t=7158be542ea76e7bd5196cefad886ddb](https://help.rackzar.com/en/download/9e1283f6c94cfd895f66c93aa3cc7cef5ffa35dbad4aa143aaf309ae9c76ac1314e3533c7b10e0ad?t=7158be542ea76e7bd5196cefad886ddb)

   Copy the private key from the text configuration from the **[Interface]** section to the **PrivateKey** field in the **WireGuard** interface settings in **Mikrotik**

   Click **OK** to create the interface.

4. Create a new Wireguard Peer

   Select the peers tab.

   Click plus to add a new peer.

   **Interface** - Select the previously created **WireGuard** interface. 

   **Public key** - Copy the public key from the text configuration from the **[Peer]** section to the Public key field.

   **Endpoint** - Copy the server **address** from the text configuration from the **[Peer]** section to the endpoint field.

   **Endpoint Port -** Copy the server **port** from the text configuration from the **[Peer]** section to the Endpoint Port field.

   **Allowed Address -** Copy **AllowedIPs** from the text configuration from the **[Peer]** section to the Allowed Address field.

   **Persistent Keepalive -** Copy the **PersistentKeepalive** from the text configuration from the **[Peer]** section to the Persistent Keepalive field.

   Click **OK** to create a peer

1. The final setup is to create the IP address that will be used for the Wireguard Tunnel.

   Select IP from the Menu then "Address"

   **Addresses -** Copy the Address from the text configuration from the **[Interface]** section to the Address field

   ** Interface** - Select the previously created **WireGuard** interface 

   Press the OK button to confirm

2. At this point your Wireguard VPN should be connected however you  will still need to add a static route to redirect our traffic over the  VPN, in this example we are routing 8.8.8.8 over the newly created  Wireguard Interface.

   Select IP, Routes and Click the + Sign to add a new static route.

   Once added you can use the traceroute within the Mikrotik to test if the traffic is leaving the VPN Wireguard Interface.

   ![7a7a54f2b7ebfd6733d0a1c304c34fb280a331912c0969dad4e163a9a6d6b34d53473f331d1e5b0a?t=31df1a2ec761041f3bbb28fc6baa9b32](https://help.rackzar.com/en/download/7a7a54f2b7ebfd6733d0a1c304c34fb280a331912c0969dad4e163a9a6d6b34d53473f331d1e5b0a?t=31df1a2ec761041f3bbb28fc6baa9b32)

3. You have now established a Wireguard client connection using Mikrotik.
