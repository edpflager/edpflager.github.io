---
title: Install MongoDB as a Service in CentOS 6.x
tags:
  - How-to
  - technical
id: '2220'
categories:
  - - Big Data
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2014-07-15 19:17:32
---

[![servicebell](http://edpflager.com/wp-content/uploads/2014/07/servicebell-298x300.png)](http://edpflager.com/wp-content/uploads/2014/07/servicebell.png)MongoDB provides a package for people to download and install on their system to try out the software, but you may want to install the software using their dedicated repository instead. Using the repository has a few advantages: MongoDB is configured as a service, so you can enable it to run when your system starts up if you like, and when newer versions of the software are released, you can update your installation via the YUM update functionality.
<!-- more -->
1.  Open a terminal session, switch to the root user and change to the /etc/yum.repos.d folder.
2.  Create a new file called mongodb.repo, add the following lines to it and save it: \[mongodb\] name=MongoDB Repository baseurl=http://downloads-distro.mongodb.org/repo/redhat/os/x86\_64/ gpgcheck=0 enabled=1
3.  To install all of the various MongoDB components on your system as well as configure a service for running the Mongo background server, enter this command: **yum install mongodb-org** (installs all of the packages below)
4.  If you don't want all of the components, you can elect to install the packages individually by running:

*   **yum install mongodb-org-server** (installs the main mongo background processes,  configuration files  and init scripts.)
*   **yum install mongodb-org-mongos** (installs the Mongos daemon that is used to locate sharded data in your MongoDB system and retrieve it.)
*   **yum install mongodb-org-shell** (installs the command line interface to the mongo server.)
*   **yum install mongodb-org-tools** (installs a number of tools for interacting with MongoDB.)

Exit the terminal prompt and you have installed MongoDB.

##### Start the MongoDB daemon/configure the service

1.  Click on System in the menu bar, and then Administration in the submenu. Click on the Service option to open the Service Configuration GUI.
2.  Scroll through the list of services until you find **mongod**. Click on it to highlight it.
3.  If you only want to run it during your current session, click on the Start icon in toolbar at the top. Otherwise, click on the Enable icon, and then the Start icon to start the service and allow it to start when your system is restarted.
4.  You'll be prompted to provide your root password to make the change. Enter it and click Authenticate to continue.
5.  Exit out of the Service Configuration window and open a terminal window.
6.  At the terminal prompt enter the command: **mongo** and you should see the MongoDB shell version (2.6.3 on mine), and a message telling you which database you are connecting to (test initially). [![mongoshell](http://edpflager.com/wp-content/uploads/2014/07/mongoshell.png)](http://edpflager.com/wp-content/uploads/2014/07/mongoshell.png)
7.  To exit the shell, enter the command: quit()

Congratulations - You have installed MongoDB as a service on your CentOS server.