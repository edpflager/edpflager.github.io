---
title: MariaDB/MySQL start on demand
tags: []
id: '1618'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2013-08-08 12:47:01
---

###### [![Mariadb](http://edpflager.com/wp-content/uploads/2013/08/Mariadb.png)](http://edpflager.com/wp-content/uploads/2013/08/Mariadb.png)Note: This method no longer works with the newer versions of Ubuntu, running systemd instead of upstart for service control. Please see the updated article [here](http://wp.me/p1BUkU-KE).

By default when MariaDB or MySQL is installed in Ubuntu, it is set to run automatically when the system starts up. For a server that makes sense, but if you are working on a development machine, like a laptop, you may not want it to always be running. (BTW MariaDB is a fork of MySQL started by one of the original MySQL developers). To disable the automatic start of MySQL on Ubuntu, open a terminal session and enter this command: **ls -l /etc/rc?.d/S\*mysql\***
<!-- more -->
This will give you a list of various runlevel folders with mysql symbolic links in them that point back to the MySQL startup scripts. (A runlevel defines how the system should start up - single user mode, multi-user with GUI but no networking, etc). You are only interested in the MySQL processes that start with an S. Go to each of those folders, and using sudo rename the file, replacing the S with a K. Once you have finished, restart your system, and MySQL won't be running. To get it to start up, open a terminal session, and enter this command: **sudo service mysql start** Another way is by using the MySQL Workbench application. Create a connection to your localhost under Server Administration. Open the connection (you'll see a screen indicating that your MySQL server is not running). Navigate to the Startup/Shutdown node under the management node, and on the right side of the screen, click the Start Server button. Your MariaDB or MySQL instance will startup.