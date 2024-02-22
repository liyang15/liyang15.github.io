---
layout: post
title:  "How to Create a Preshared Key for Wireguard"
categories: netdevops
tags: Network Engineer
date: 2024-2-22
---

You may have encountered a Mikrotik error when trying to create preshared key

```
Couldn't change wireguard peer<> - invalid preshared key (6)
```

This is because a Wireguard preshared key needs to be 256bit (32  byte) base64 encoded key. We have a couple different ways we can  generate the correct format.

\1. Use Openssl to generate a random 32 byte password

```
openssl rand 32 | base64
```

\2. Create a 31 character password and base64 encode it

```
echo Thisisthepassword31characterslo | base64
VGhpc2lzdGhlcGFzc3dvcmQzMWNoYXJhY3RlcnNsbwo=
```

Now we can take this and add it to our config. The config option is

```
PresharedKey = VGhpc2lzdGhlcGFzc3dvcmQzMWNoYXJhY3RlcnNsbwo=
```

https://www.wireguard.com/papers/wireguard.pdf

https://forum.mikrotik.com/viewtopic.php?t=184469
