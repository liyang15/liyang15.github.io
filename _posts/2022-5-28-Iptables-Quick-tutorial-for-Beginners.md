---
layout: post
title:  "Iptables: Quick tutorial for Beginners"
categories: netdevops
tags: Network Engineer
date: 2022-5-28
---


# Iptables: Quick tutorial for Beginners

Iptables is a firewall mostly included in Linux distribution to secure desktops from malicious requests. Although GUI version Firestarter is also available, iptables is also not much difficult to learn once you know the basic commands. This is a basic tutorial for beginners. In this tutorial, we will see an installation of iptables and later some basic commands to block/allow unwanted traffic. [Click Here if you are interested to learn about another Linux-based firewall, Uncomplicated firewall or ufw](https://allabouttesting.org/quick-tutorial-how-to-configure-the-uncomplicated-firewall-on-linux/).

Just remember default Linux firewall is Netfilter not iptables. Netfilter is available in every Linux-based distro by default.

**Advantages of iptables as a firewall**

- pre-installed on most Linux based distro
- very well documented
- absolutely free
- easy to use
- provide flexibility to set up simple rules to configure VPN

**Disadvantages of using iptables as a firewall**

- If you want to add any new rule to an existing iptables firewall, the entire rule needs to be reloaded.
- Not compatible with IPV6. IPV6 can be implemented using ip6tables.

**Tables of iptables**

| **Filter table**    | **Mangle table**                  | **Security table**                              | **Net Address Translation (NAT) table** | **Raw table**                    |
| ------------------- | --------------------------------- | ----------------------------------------------- | --------------------------------------- | -------------------------------- |
| default of iptables | alter the attribute of IP headers | Mandatory Access Control (MAC) networking rules | implement NAT rules                     | configuring exemption in ruleset |

**Installation** 

It is pre-installed in most Linux distributions. Although, you can install iptables by using the below command:

```
#sudo apt-get install iptables
```

[![img](data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7)](https://i0.wp.com/allabouttesting.org/wp-content/uploads/2018/02/install-iptables.jpg?ssl=1)



**To uninstall iptables, you can use the below command:**

```
#sudo apt-get autoremove iptables
```

[![img](data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7)](https://i0.wp.com/allabouttesting.org/wp-content/uploads/2018/02/sudo-apt-get-autoremove-iptables.jpg?ssl=1)



**Types of chains**

Iptables is categorized into three types of chains:

**1) INPUT:** Chain is used to control the flow of incoming traffic. Suppose your friend Tom wants to SSH into your laptop, iptables use INPUT chain to match the IP address and port.

```
#iptables -A INPUT -s xx.xx.xx.xx -j DROP
```

**2) OUTPUT:** Chain is used to control the outgoing flow from the machine. If you plan to ping http://allabouttesting.org, iptables will check the configuration related to the target and accordingly allow or block the connection attempt.

```
#iptables -A OUTPUT -s xx.xx.xx.xx -j DROP
```

**3) FORWARD:** It allows an administrator to control the traffic that can be routed within a LAN.

```
#iptables -P FORWARD DROP
```

**Basic commands for iptables**

Now we will discuss some basic commands which are frequently used by Linux user to configure iptables.

**To check whether iptables is installed or not**

```
#dpkg -s iptables
```

[![img](data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7)](https://i0.wp.com/allabouttesting.org/wp-content/uploads/2018/02/verify-iptables.jpg?ssl=1)



**To list out all available options or commands**

```
#iptables -h
```

[![img](data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7)](https://i0.wp.com/allabouttesting.org/wp-content/uploads/2018/02/help.jpg?ssl=1)



**To list out all rules configured by iptables**

This command list out all policies which implemented by iptables. If you need to check whether iptables is blocking a port, use the below command:

```
#iptables -L
```

[![img](data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7)](https://i0.wp.com/allabouttesting.org/wp-content/uploads/2018/02/List-out-rules.jpg?ssl=1)



You can also use "#iptables -L -vn" to list out details in terms of port number, instead of its name

[![img](data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7)](https://i0.wp.com/allabouttesting.org/wp-content/uploads/2018/03/iptables-list-rules.jpg?ssl=1)



**To implement default drop policy for INPUT, OUTPUT, and FORWARD**

```
#iptables -P INPUT DROP
#iptables -P OUTPUT DROP
#iptables -P FORWARD DROP
```

[![img](data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7)](https://i0.wp.com/allabouttesting.org/wp-content/uploads/2018/02/iptables-DROP-policy-.jpg?ssl=1)



**To delete all rules configured by iptables**

```
#iptables  -F
```

**Block all connections from particular IP** 

```
#iptables -A INPUT -s 192.168.0.89 -j DROP
```

**Block all connections** from a **range of IPs**

```
 #iptables -A INPUT -s 192.168.0.0/24 -j DROP
```

**Block all connections from a specific MAC address**

```
#iptables -A INPUT -m mac --mac-source xx.xx.xx.xx.xx.xx -j DROP
```

**Note:** Use "ipconfig/all" for Windows and "ifconfig -a" for Linux to identify the MAC address of the machine.

**Conclusion**

This is not the end of learning. Still, many more options are available to configure iptables, which helps in controlling network packets. Explore the internet to learn more features and commands of iptables.

Subscribe us to receive more such articles updates in your email.

If you have any questions, feel free to ask in the comments section below. Nothing gives me greater joy than helping my readers!

**Disclaimer:** This tutorial is for educational purpose only. Individual is solely responsible for any illegal act.