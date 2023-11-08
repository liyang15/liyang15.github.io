---
layout: post
title:  "VyControl - Web UI for VyOS Firewall"
categories: netdevops
tags: Network Engineer
date: 2023-11-8
---

# VyControl - Web UI for VyOS Firewall

[VyControl](https://github.com/vycontrol/vycontrol) project is a single frontend interface to manage a single or multiple VyoS servers. It was developed by Roberto Berto and is written in Django/Python. It currently supports firewall and static routes configuration. Additional features are planned such as IPSEC, openvpn and basic dynamic routing.

My goal is to provide easy-to-reproduce installation steps so you can quickly test if VyControl is working as expected. So far, I have only been successful with static route configuration of the VyOS instance 1.4-rolling, firewall configuration does not work for me. I have tested VyOS instance version 1.3-beta as well, but achieved the same result. Therefore, I consider VyControl to be an interesting prototype rather than a project that can be used in production. However, I really like the idea having a single GUI interface for managing multiple instances of VyOS and the project is a nice attempt to achieve this goal.

Firstly, we are going to enable HTTPS API on VyOS instance. VyOS

**1. HTTPS API Configuration on VyOS Router**

VyControl requires the latest VyOS API (VyOS version 1.3+ rolling release) supported by the latest VyOS [rolling release](https://downloads.vyos.io/rolling/current/amd64/vyos-rolling-latest.iso) ISO image. You can use my deploy and installation scripts written for Linux to install ISO on VMDK image. To do so, Qemu hypervisor must be installed The scripts are available in [Automatic Deployment VyOS Live ISO on VMware VM](https://brezular.com/2016/10/18/scripts/) section.

vyos@vyos:~$ configure
[edit]
vyos@vyos# set service https api keys id my_id key 'my_secret_key'
vyos@vyos# set service https certificates system-generated-certificate lifetime '65535'
vyos@vyos# set service https virtual-host vyos1 listen-address '172.17.100.99'
vyos@vyos# set service https virtual-host vyos1 listen-port '6443'
vyos@vyos# set service https virtual-host vyos1 server-name 'vyos1.example.com'

vyos@vyos# commit
vyos@vyos# save
vyos@vyos# exit

Nginx web server is now listening on the socket 172.17.100.99:6443.

![img](https://i0.wp.com/brezular.com/wp-content/uploads/2021/03/Picture1-Nginx_6443.png?resize=640%2C37&ssl=1)

Picture 1 - **VyOS HTTPS API Activated**

**2. VyControl Installation**

The fastest way to test a VyControl project is to download the VyControl Docker image and launch the container (section 2.1). However, we will also discuss the manual installation method, which involves deploying VyControl on a VirtualBox machine with Debian 10 Linux installed (Part 2.2).

**2.1 Deploying VyControl Docker Container**

Assuming that Docker is already installed, enter the command below to pull VyControl image from the repository.

$ docker pull robertoberto/vycontrol

Rund docker container:

$ docker run -p 8000:8000 -t robertoberto/vycontrol

You can access web interface at http://127.0.0.1.![img](https://i0.wp.com/brezular.com/wp-content/uploads/2021/03/Picture2-Network_Topology_Container_VyControl.png?resize=404%2C174&ssl=1)

Picture 2 - **VyControl Docker Container**

**2.2 VyControl Installation on Debian 10**

Let's start VyControl installation, we are in a directory /home/debian/.![img](https://i0.wp.com/brezular.com/wp-content/uploads/2021/03/Picture3-Network_Topology_VyControl_VM.png?resize=396%2C173&ssl=1)

Picture 3 - **VyControl Installed on Debian 10 Linux VirtualBox Virtual Machine**

Download and install required packages.

$ sudo apt install python3-pip git python3-venv

Note: Venv is a package that comes with Python 3. We will use it to create our virtual environment. Virtualenv is a library that offers more functionality than venv. In case you prefer virtualenv to venv, install virtualenv with the command:

$ sudo pip3 install virtualenv

Clone VyControl repository.

$ git clone https://github.com/vycontrol/vycontrol.git

Setup Python virtual env and pip requirements:

To create a virtual environment, go to vycontrol project’s directory and run venv. The argument of the venv is the name of directory (my_vycontrol_env) where the virtual environment files are stored.

$ cd /home/debian/vycontrol

$ python3 -m venv /home/debian/vycontrol/my_vycontrol_env

Venv will create a virtual Python installation in the /home/debian/vycontrol/my_vycontrol_env folder.

Note: To create virtual environment with virtualenv:

$ virtualenv /home/debian/vycontrol/my_vycontrol_env

Activate a virtual environment:

$ source /home/debian/vycontrol/my_vycontrol_env/bin/activate

Note: If you want to switch projects or otherwise leave your virtual environment, simply run:

$ deactivate

Install requirements in the file /home/debian/vycontrol/requirements.txt.

$ pip3 install -r requirements.txt

Note: if you get an error message 'Failed building wheel for python-slugify', simply add the wheel package to the file requirement.txt and rerun command installation of requirements.

Setup initial database:

$ cd vycontrol
$ python3 manage.py migrate

Run Django webserver:

$ python3 manage.py runserver

Open web browser and navigate to http://127.0.0.1:8000.

**3. VyControl Configuration**

Create user used to access VyControl site. Add a new VyOS instance to VyControl. Fill the required fields (Picture 4) and click Add Instance.

![img](https://i0.wp.com/brezular.com/wp-content/uploads/2021/03/PIcture4-Creating_VyOS_Instance.png?resize=640%2C412&ssl=1)

Picture 4 - **Adding New VyOS Instance to VyControl**

Click 'test' in the test connection column (Picture 5).

![img](https://i0.wp.com/brezular.com/wp-content/uploads/2021/03/PIcture5-Testing_Connection_to_VyOS.png?resize=640%2C220&ssl=1)

Picture 5 - **Checking Connection to VyOS Instance vyos-I 172.17.100.2**

If everything works OK, you should see an Instance Connected message. (Picture 6).

![img](https://i0.wp.com/brezular.com/wp-content/uploads/2021/03/Picture6-Connected-_Instance-Vyos-I.png?resize=640%2C219&ssl=1)

Picture 6 - **Instance Vyos-I is Reachable**

**4. VyControl Static Routing and Firewall COnfiguration Testing**

According to our test, static routing is working ok. It means that we can add / delete static routes using VyControl web interface (Picture 7).

![img](https://i0.wp.com/brezular.com/wp-content/uploads/2021/03/Picture7-Static_Route_Configuration.png?resize=640%2C125&ssl=1)

Picture 7 - **Static Route 192.168.1.0/24 Configured from VyControl Web UI**

The Picture 8 depicts the screenshot from VyOS CLI, displaying our new static route 192.168.1/0/24.

![img](https://i0.wp.com/brezular.com/wp-content/uploads/2021/03/Picture8-VyOS_Static_Route_Configuration.png?resize=640%2C148&ssl=1)

Picture 8 - **Displaying IPv4 Static Routing Information on VyOS** 

However, the firewall configuration displays an error message (Picture 9).

![img](https://i0.wp.com/brezular.com/wp-content/uploads/2021/03/Picture9-Furewall_Configuration_Error.png?resize=640%2C344&ssl=1)

Picture 9 - **Firewall Configuration Error**

The error message is displayed regardless of whether the VyControl was deployed as Docker image or installed manually.

End.
