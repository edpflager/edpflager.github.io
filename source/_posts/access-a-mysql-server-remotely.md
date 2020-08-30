---
title: Access a MySQL Server Remotely
tags:
  - cookbook
  - ETL
  - guides
  - How-to
  - howto
  - kettle
  - MySQL
  - PDI
  - SysAdmin
  - technical
  - Ubuntu
id: '3316'
categories:
  - - Business Intelligence
  - - Linux
comments: false
date: 2016-05-31 04:49:13
---

[![logo-mysql](http://edpflager.com/wp-content/uploads/2014/11/logo-mysql.png)](http://edpflager.com/?attachment_id=2588#main)A quick one today: While working on a project, I couldn't access the MySQL server (version 5.7.12) that was on another system. I was in a development environment on a local network with just me on in, so the MySQL server did not have a firewall running. Here is what I did to get my connection to work.

1.  Add an Administrator user account with permissions to connect from any host:
    
    **`CREATE USER 'edpflager'@'%' IDENTIFIED BY 'my_password';`** **`GRANT ALL PRIVILEGES ON *.* TO 'edpflager'@'%'`** **`WITH GRANT OPTION;`**
    
2.  Next open a terminal prompt on the MySQL server, and navigate to /etc/mysql/mysql.conf.d
3.  Open a text editor as superuser  and edit mysqld.cnf
    
    sudo nano ./mysqld.cnf
    
4.  Find the following line and add a # to the beginning to comment it out:
    
    bind-address = 127.0.0.1
    
5.  Save, exit, and restart MySQL to make it take effect.

