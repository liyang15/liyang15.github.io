---
layout: post
title:  "phpIPAM installation on Debian 6.0.6"
categories: netdevops
tags: Network Engineer
date: 2024-2-27
---

I have received a request for help on manual installation of phpIPAM on debian linux, so I decided to write a small how-to if anyone else has problems or is not so familiar with linux distributions and environment. I have used fresh default debian 6.0 as distribution because it is widely used, I believe on ubuntu linux procedure should be very similar, except maybe for locations of some config files.

I have used the following settings for installation:

- Fresh debian installation
- MySQL server not yet installed and no root pass configured
- Apache not installed and configured
- phpipam will be installed in default directory (no vhosts) under /phpipam/ folder

If you already have MySQL/apache set you can skip point 3.

Installation procedure:



### 1.) Preparing environment and installing required apps

Update your sources (apt-get update) and install Apache, php and mysql server:

```
apt-get install apache2 mysql-server php5 php5-gmp php-pear php5-mysql php5-ldap
```

After all is installed and the apache server is running, you need to decide weather you will be running it under vhost or in subdirectory or root directory. For this guide I will have it in subdirectory http://server/phpipam/, so do the following:

```
cd /var/www/
wget http://freefr.dl.sourceforge.net/project/phpipam/phpipam-1.1.010.tar
tar -xvf phpipam-1.1.010.tar
rm phpipam-1.1.010.tar
cd phpipam
```

At this point you should have all the files and applications installed for phpipam.



### 2.) Setting phpipam configuration

Copy config.dist.php to config.php and edit it. Set MySQL user/pass you wish to use for phpipam and set rewriteBase to /phpipam/,and set your database details (select user/pass you wish to have for phpipam and database name – you can also leave it as it is):

```
vi config.php
$db['host'] = "localhost";
$db['user'] = "phpipam";
$db['pass'] = "phpipamadmin";
$db['name'] = "phpipam";
```

also set rewrite base in config.php (If you have it under root directory of your webserver than you can skip it)

```
define('BASE', "/phpipam/");
```



### 3.) Setting up database

Now you need to setup MySQL database, if this is the first time you installed it (if you already have mysql running with root pass then you can skip this):

```
mysqladmin -u root password NEWPASSWORD
```



### 4.) Mod rewrite settings (optional)

Make sure apache supports mod_rewrite for web server, if it does not you will get errors /install/ not found (http 404) and similar. This means that either apache is not set to use .htaccess rewrites, or that you did not specify the rewritebase in step2 correctly.

Search for Directory directive in default apache config and add/change it to

```
vi /etc/apache2/sites-enabled/000-default
    Options FollowSymLinks
    AllowOverride all
    Order allow,deny
    Allow from all
```

On debian you have to manually enable rewrite mod for apache, you can do it manually or via a2enmod

```
ln -s /etc/apache2/mods-available/rewrite.load /etc/apache2/mods-enabled/rewrite.load
```

or

```
sudo a2enmod rewrite
```

And restart apache:

```
/etc/init.d/apache2 restart
```



### 5.) Database installation

Now point you web browser to http://server/phpipam/ and the install script will start that installs default schema for phpipam database. Use mysql root password that you created in step 3 and click install.
