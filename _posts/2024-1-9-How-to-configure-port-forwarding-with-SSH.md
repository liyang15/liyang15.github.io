---
layout: post
title:  "How to configure port forwarding with SSH"
categories: netdevops
tags: Network Engineer
date: 2024-1-9
---

# How to configure port forwarding with SSH

Besides using [SSH](https://networklessons.com/tag/ssh) to connect to (Linux) servers or network equipment, you can also use it to tunnel traffic. It works the same way as a [VPN](https://networklessons.com/tag/vpn) as you can reach all hosts behind the SSH server. Besides being able to reach hosts behind the SSH server, our traffic is also secure because SSH will encrypt it. Let me show you a picture to explain this:



![Ssh Tunnel](https://cdn.networklessons.com/wp-content/uploads/2013/02/ssh-tunnel.png)



Above, you see an SSH client on the left side. This computer is on our LAN and has IP address 192.168.1.1. The most popular SSH client for Windows is [putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html), and you can download it for free. If you are using Linux or a Mac, you can use SSH from the command line. On the right side, we have an SSH server with public IP address 1.2.3.4. In my picture, this is a router, but you can run an SSH server on many devices, including Windows, Linux, Mac, routers, and storage devices from Synology or Qnap. On the right side, there’s also a web server with IP address 192.168.2.1.

We can connect to the router using SSH, but perhaps you didn’t know it’s also possible to reach the webserver through the SSH tunnel! Let me show you how to do this with putty:



![Putty Start Screen](https://cdn.networklessons.com/wp-content/uploads/2013/02/putty-start-screen.png)



First, I will type in the IP address of the SSH server, but before we click on “open”, we’ll configure SSH tunneling. Take a look at the screenshot below:



![Putty Ssh Port Forward](https://cdn.networklessons.com/wp-content/uploads/2013/02/putty-ssh-port-forward.png)



Click on “connection” to expand it and select “SSH”. Finally, select “Tunnels” to configure SSH port forwarding. I want to reach the web server, so the destination will be IP address 192.168.2.1 and port 80. You can pick any source port that you like. I chose the number 5000. Click on the “Add” button, and it will look like this:



![Putty Forwarded Ports](https://cdn.networklessons.com/wp-content/uploads/2013/02/putty-forwarded-ports.png)



It will show you which ports will be forwarded. Now hit the “Open” button and type in the username and password that you normally use. You will see the normal SSH login screen, but our port will be forwarded in the background. Let’s open a web browser and connect to the webserver:



![Ssh Webtraffic Forwarded](https://cdn.networklessons.com/wp-content/uploads/2013/02/ssh-webtraffic-forwarded.png)



I’ll type in “localhost,” which refers to my local computer, and port number 5000, the source port I chose for the port forwarding in putty. As you can see, it connects to the webserver! Whenever you connect to localhost port 5000, it will be forwarded to IP address 192.168.2.1 port 80.

The cool thing about SSH is that you only need to open one single port, and by tunneling traffic like this, you can reach any device behind the SSH server. Everything it sends through this SSH tunnel is safe because it’s encrypted.

If you are using Linux or MAC, you don’t have to use putty, but you can use the command line, which is a lot easier:

```
sudo ssh -L 5000:192.168.2.1:80 1.2.3.4
```

The above line is similar to the configuration I showed you in Putty. We will connect our local port 5000 to the remote web server (192.168.2.1 port 80) and connect to SSH server 1.2.3.4.

That’s all I wanted to show you! If you enjoyed this lesson, please leave a comment.
