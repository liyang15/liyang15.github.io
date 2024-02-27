---
layout: post
title:  "phpipam installation guide"
categories: netdevops
tags: Network Engineer
date: 2024-2-27
---

phpipam is easy to install, please read below guide to help you setup phpipam!



Here are some customized guides for your distribution:

- [phpIPAM installation on CentOS 7](https://phpipam.net/phpipam-installation-on-centos-7/)
- [phpIPAM installation on Debian 6.0.6](https://phpipam.net/phpipam-installation-on-debian-6-0-6/)



## 1.) Requirements

before you start installing phpipam, please make sure you meet following requirements:

1. Apache2 webserver with php support or Nginx with php-fpm
2. Mysql server (5.1+)
3. PHP:
   - version 5.3 supported to phpipam version 1.3.1
   - version 5.4
   - version 7.2 and higher supported from phpipam release 1.3.2
4. PHP modules:
   - pdo, pdo_mysql : Adds support for mysql connections
   - session : Adds persistent session support
   - sockets : Adds sockets support
   - openssl : Adds openSSL support
   - gmp : Adds support for dev-libs/gmp (GNU MP library) -> to calculate IPv6 networks
   - ldap : Adds LDAP support (Lightweight Directory Access Protocol – for AD also)
   - crypt : Add support for password encryption
   - SimpleXML: Support for SimpleXML (optional, for RIPE queries and if required for API)
   - json: Enable JSON support
   - gettext: Enables translation
   - filter : Adds filtering support
   - pcntl : Add support for process creation functions (optional, required for scanning)
   - cli : Enable CLI (optional, required for scanning and status checks)
   - mbstring : Enable mbstring support
5. php PEAR support

Usually most php modules all are built into default php installation. If some required modules are missing phpipam will fail with warning and notify you about them.

You can check which php modules are enabled by issuing `php -m` in command line.

## 2.) Downloading phpipam

You can download and install phpipam from official [Sourceforge](https://sourceforge.net/projects/phpipam/) repository and extract it to your webserver directory.

```
tar -xvf phpipam-1.6.tar /var/www/
```

Or Simply use GitHub:

```
[root@ipam /]# GIT clone --recursive https://github.com/phpipam/phpipam.git /var/www/phpipam
[root@ipam /]# cd /var/www/phpipam
[root@ipam /var/www/phpipam]# git checkout -b 1.6 origin/1.6
```

You can find other methods in [Downloads](https://phpipam.net/download/) page.

## 3.) Initial configuration

Before you start installing database files, you need to enter database details, that you will use for phpipam connecting to database. First copy config.dist.php to config.php and enter required details.

For automatic installation phpipam will configure database with settings you enter in config.php file, for manual installation you will have to do it yourself.

```
$db['host'] = "localhost";
$db['user'] = "phpipam";
$db['pass'] = "phpipamadmin";
$db['name'] = "phpipam";
```

also, if you extracted phpipam directory in any other directory than web server root folder, you need to set that as well (BASE directive) in config.php:

```
 define('BASE', "/");
```

For example, if you will have phpipam installed in `http://myserver/phpipam/` directory than set BASE as /phpipam/.



## 4.) Database installation

You can install required database scheme in 3 ways:

### a) Automatic database installation

On phpipam installation page select automatic installation, enter mysql details and click install. For mysql user/pass enter details for user, that has permissions to create new databases and grant permission (e.g. root).

### b) mysql import

You can manually import sql SCHEMA file via mysql’s cli, but first you need to create database and grant user permission (replace user/pass with one you set in config.php):

```
[root@ipam ~]# mysql -u root -p
Enter password:
mysql> create database phpipam;
Query OK, 1 row affected (0.00 sec)
mysql> GRANT ALL on phpipam.* to phpipam@localhost identified by ‘phpipamadmin’;
Query OK, 0 rows affected (0.00 sec)
mysql> exit
Bye
```

Once this is in place, you can import SCHEMA.sql file with following command:

```
mysql -u root -p phpipam < db/SCHEMA.sql
```

### c) manual query import

Third method is a copy/paste method of SQL queries. First create database and grant user permission (take a look at step b), then copy contents of SCHEMA.sql file into mysql and you should be good to go.

This method is useful in case you have some problems with installation, as you can see where it goes wrong!

## 5.) Database backups (optional)

It is recommended that you make daily backups of your database files via cron. For daily backups use following cronjob:

```
# Backup IP address table, remove backups older than 10 days
@daily /usr/bin/mysqldump -u ipv6 -pipv6admin phpipam > <ipam_dir>/db/bkp/phpipam_bkp_$(date +"\%y\%m\%d").db
@daily /usr/bin/find <ipam_dir>/db/bkp/ -ctime +10 -exec rm {} \;
```

## 6.) URL layout

To change URL layout for phpipam from **http://phpipam.net/?page=subnets&sectionId=2** to **http://phpipam.net/subnets/2/** please check [this](https://phpipam.net/documents/prettified-links-with-mod_rewrite/) guide.

## 7.) NGINX support

Please take a look at https://phpipam.net/news/phpipam-on-nginx/
