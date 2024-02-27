---
layout: post
title:  "How To Install phpIPAM on CentOS 8  RHEL 8"
categories: netdevops
tags: Network Engineer
date: 2024-2-27
---

Today’s guide will be on how to install phpIPAM on CentOS 8 / RHEL 8 Linux distribution. phpIPAM is the leading open-source web-based tool for IP address management application (IPAM). phpIPAM is written in PHP to provide a modern and useful IP address management features. This tool jQuery libraries, Ajax and HTML5/CSS3 with MySQL being a database backend.

#### Features of phpIPAM

Here are the top features of phpIPAM.

- IPv4/IPv6 IP address management
- Section / Subnet management
- Automatic free space display for subnets
- Visual subnet display
- Automatic subnet scanning / IP status checks
- PowerDNS integration
- NAT support
- VLAN management
- VRF management
- IPv4 / IPv6 calculator
- IP database search
- E-mail notifications
- Custom fields support
- Translations
- Changelogs
- RACK management
- Domain authentication (AD, LDAP, Radius)
- Per-group section/subnet permissions
- Device/device types management
- RIPE subnets import
- XLS / CVS subnets import
- IP request module
- REST API
- Locations module

## Install phpIPAM on CentOS 8 / RHEL 8



phpIPAM has a number of dependencies that we’ll need to install first.



1. MySQL / MariaDB database server.
2. PHP / PHP-FPM
3. A number of PHP extensions
4. A web server – Apache / Nginx

In this installation of phpIPAM on CentOS 8 / RHEL 8, we’ll use Apache as our preferred web server.

### Step 1: Install httpd and PHP

Let’s kickoff with the installation of a web server (Apache httpd), PHP and required PHP extensions.

```
sudo dnf -y install @httpd
sudo dnf -y install @php
sudo dnf -y install  php-{mysqlnd,curl,gd,intl,pear,mbstring,gettext,gmp,json,xml,fpm}
```

Start and enable both httpd and php-fpm services.

```
sudo systemctl enable --now httpd php-fpm
```

Confirm status returns *running*.

[![install phpfpm centos rhel 8 start nginx](https://computingforgeeks.com/wp-content/uploads/2019/10/install-phpfpm-centos-rhel-8-start-nginx-1024x635.png?ezimgfmt=rs:696x432/rscb23/ng:webp/ngcb23)](https://computingforgeeks.com/wp-content/uploads/2019/10/install-phpfpm-centos-rhel-8-start-nginx.png?ezimgfmt=rs:696x432/rscb23/ng:webp/ngcb23)

### Step 2: Install MariaDB Database server

The next step is the installation of MariaDB. Follow the guide below to install it.

- [Install MariaDB on CentOS / RHEL 8](https://computingforgeeks.com/how-to-install-mariadb-database-server-on-rhel-8/)

Once the installation is done, login to MySQL CLI as root user and create phpIPAM database and user.

```
$ mysql -u root -p
CREATE DATABASE phpipam;
GRANT ALL ON phpipam.* TO phpipam@localhost IDENTIFIED BY 'IpamStr0ngP@sswOrd';
FLUSH PRIVILEGES;
QUIT;
```

### Step 3: Install phpIPAM on CentOS 8 / RHEL 8

Pull the latest source code of phpIPAM from the Github repository.

```
sudo dnf -y install git
sudo git clone --recursive https://github.com/phpipam/phpipam.git /var/www/html/phpipam
```

Configure phpIPAM:

```
cd /var/www/html/phpipam
```

Copy **config.dist.php** to **config.php**.

```
sudo cp config.dist.php config.php
```

Edit the file:

```
sudo vi config.php
```

Configure database credentials as added on **Step 2:**

```
/**
* database connection details
******************************/
$db['host'] = 'localhost';
$db['user'] = 'phpipam';
$db['pass'] = 'IpamStr0ngP@sswOrd';
$db['name'] = 'phpipam';
$db['port'] = 3306;
```

### Step 4: Configure Apache web server

Create an Apache httpd configuration file for phpIPAM on CentOS 8 / RHEL 8.

```
sudo vi /etc/httpd/conf.d/phpipam.conf
```

Add:

```
<VirtualHost *:80>
    ServerAdmin admin@example.com
    DocumentRoot "/var/www/html/phpipam"
    ServerName phpipam.computingforgeeks.com
    ServerAlias www.phpipam.computingforgeeks.com
    <Directory "/var/www/html/phpipam">
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
    ErrorLog "/var/log/httpd/phpipam-error_log"
    CustomLog "/var/log/httpd/phpipam-access_log" combined
</VirtualHost>
```

Change ownership of the /var/www/phpipam directory to www-data user and group.

```
sudo chown -R apache:apache /var/www/html/
```

Validate your httpd configurations.

```
$ sudo apachectl -t
Syntax OK
```

If all looks good, restart httpd service.

```
sudo systemctl restart httpd
```

Status should indicate running without any errors.

```
$ systemctl status httpd
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; vendor preset: disabled)
  Drop-In: /usr/lib/systemd/system/httpd.service.d
           └─php-fpm.conf
   Active: active (running) since Wed 2023-08-16 11:10:11 UTC; 1h 23min ago
     Docs: man:httpd.service(8)
 Main PID: 7795 (httpd)
   Status: "Total requests: 136; Idle/Busy workers 100/0;Requests/sec: 0.0273; Bytes served/sec: 2.5KB/sec"
    Tasks: 278 (limit: 10843)
   Memory: 35.5M
   CGroup: /system.slice/httpd.service
           ├─7795 /usr/sbin/httpd -DFOREGROUND
           ├─7796 /usr/sbin/httpd -DFOREGROUND
           ├─7797 /usr/sbin/httpd -DFOREGROUND
           ├─7798 /usr/sbin/httpd -DFOREGROUND
           ├─7799 /usr/sbin/httpd -DFOREGROUND
           └─8543 /usr/sbin/httpd -DFOREGROUND

Aug 16 11:10:11 cent8.mylab.io systemd[1]: Starting The Apache HTTP Server...
Aug 16 11:10:11 cent8.mylab.io systemd[1]: Started The Apache HTTP Server.
Aug 16 11:10:11 cent8.mylab.io httpd[7795]: Server configured, listening on: port 80
```

### Step 5: Finish phpIPAM Installation

Open the Server domain URL on [http://domain.com](https://computingforgeeks.com/), replace [domain.com](https://computingforgeeks.com/) with your valid domain name.

![phpipam install 01](https://computingforgeeks.com/wp-content/uploads/2018/06/phpipam-install-01.png?ezimgfmt=rs:696x389/rscb23/ng:webp/ngcb23)

Select “**New phpipam installation**“. On the next page, choose the database installation method.

![phpipam install 02](https://computingforgeeks.com/wp-content/uploads/2018/06/phpipam-install-02.png?ezimgfmt=rs:696x445/rscb23/ng:webp/ngcb23)

Select **MySQL import instructions**. This will print the command to import SQL file.

```
sudo mysql -u root -p phpipam < /var/www/html/phpipam/db/SCHEMA.sql
```

When done, click the **login** button.

![phpipam install 08](https://computingforgeeks.com/wp-content/uploads/2018/06/phpipam-install-08.png?ezimgfmt=rs:696x355/rscb23/ng:webp/ngcb23)

The default Login credentials are:

```
Username: admin
Password: ipamadmin
```

You’re prompted to change the admin password on the first login.

[![install phpfpm centos rhel 8 change password](https://computingforgeeks.com/wp-content/uploads/2019/10/install-phpfpm-centos-rhel-8-change-password.png?ezimgfmt=rs:696x319/rscb23/ng:webp/ngcb23)](https://computingforgeeks.com/wp-content/uploads/2019/10/install-phpfpm-centos-rhel-8-change-password.png?ezimgfmt=rs:696x319/rscb23/ng:webp/ngcb23)

The phpIPAM installation on CentOS 8 / RHEL 8 has been completed successfully.

![phpipam install 09](https://computingforgeeks.com/wp-content/uploads/2018/06/phpipam-install-09.png?ezimgfmt=rs:696x323/rscb23/ng:webp/ngcb23)

You can start adding Subnets or use the discovery feature to pull subnets available in your network. phpIPAM is one of my favorite Network Management tool. I hope you enjoy using it.

***Recommended Linux Books to read:\***

- [Best Linux Books for Beginners & Experts](https://computingforgeeks.com/best-rated-top-linux-books-for-beginners/)
- [Best Linux Kernel Programming Books](https://computingforgeeks.com/best-linux-kernel-programming-books/)
- [Best Linux Bash Scripting Books](https://computingforgeeks.com/best-linux-bash-scripting-books-of-all-time/)
- [Top RHCSA / RHCE Certification Study Books](https://computingforgeeks.com/top-rhcsa-rhce-certification-study-books/)
- [Best Top Rated CompTIA A+ Certification Books](https://computingforgeeks.com/best-top-rated-comptia-certification-books/)
- [Best LPIC-1 and LPIC-2 certification study books](https://computingforgeeks.com/best-lpic-1-and-lpic-2-certification-study-books/)
