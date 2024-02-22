---
layout: post
title:  "Quick intro to setting up Wireguard interfaces in Mirktok’s RouterOS"
categories: netdevops
tags: Network Engineer
date: 2024-2-22
---

Recently I’ve been using Wireguard to fix the things I once used IPsec for.

It was merged into the Mainline Linux kernel late last year, and then in RouterOS 7.0beta7 (2020-Jun-3) the system kernel on RouterOS was  upgraded to version 5.6.3 which contains Wireguard support.

Unfortunately this feature is going to stay in the Unstable /  Development releases for the time being until a kernel update is done  for the stable release to 5.5.3 or higher, but for now I thought I’d try it out.

After loading a beta version of the firmware, under Interfaces I have the option to add a Wireguard interface, for clients to connect to my  Mikrotik using Wireguard.

![img](https://i0.wp.com/nickvsnetworking.com/wp-content/uploads/2021/09/image-1.png?resize=272%2C403&ssl=1)

It’s nice and simple to see the public/private key pair (a new key  pair is generated for each Wireguard instance which is nifty) that we an use to authenticate / be authenticated.

![img](https://i0.wp.com/nickvsnetworking.com/wp-content/uploads/2021/09/image-2.png?resize=558%2C438&ssl=1)

If we want to configure remote peers, we do this by jumping over to  the Wireguard -> Peers tab, allowing us to setup Peers from here.

![img](https://i0.wp.com/nickvsnetworking.com/wp-content/uploads/2021/09/Wireguard-Peer-Setup.gif?resize=580%2C399&ssl=1)

Obviously routing and firewalls remain to be setup, but I love the  simplicity of Wireguard, and in the RouterOS implimentation this is  kept.

