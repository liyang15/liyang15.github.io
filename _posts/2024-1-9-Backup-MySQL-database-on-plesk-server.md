---
layout: post
title:  "Backup MySQL database on plesk server"
categories: netdevops
tags: Network Engineer
date: 2024-1-9
---

# Backup MySQL database on plesk server

If you want to make a backup from an MYSQL Database on a Plesk server you can do this from the command line. There’s just one catch…you need to be careful that you can’t specify the username/password yourself. Instead, you need to use the following command:

```
mysql -uadmin -p`cat /etc/psa/.psa.shadow`
```

This will refer to the PSA shadow file for access.

For example, if you want to backup a database this is what you should do:

```
mysqldump -uadmin -p`cat /etc/psa/.psa.shadow` databasename > backupfile.sql
```

That’s all there is to it!
