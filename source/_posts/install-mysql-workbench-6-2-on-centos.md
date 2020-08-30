---
title: Install MySQL Workbench 6.2 on Centos
tags:
  - centos
  - ETL
  - How-to
  - howto
  - install
  - MySQL
  - SysAdmin
  - technical
id: '2578'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2014-11-28 19:12:11
---

[![workbench](http://edpflager.com/wp-content/uploads/2014/11/workbench-300x102.png)](http://edpflager.com/wp-content/uploads/2014/11/workbench.png)The world of computers is constantly evolving, and that means having to upgrade your software periodically if you want to stay current. The GA version of MySQL Workbench, the GUI tool for interacting with the MySQL database engine was recently updated. For information on changes, you can check out the official documentation at this [link](http://dev.mysql.com/downloads/workbench/), but a couple of the biggest changes revolve around Microsoft products:

*   you can now migrate Microsoft Access databases, and
*   64-bit Windows binaries are now provided to go along with the 32-bit ones.

I use MySQL as a test bed for a lot of Pentaho development, so I like to keep the related tools up to date. Although this version does work with Centos 6.6 (the version I am using of the RHEL  distribution), its not as easy as it should be to install.
<!-- more -->
To get MySQL Workbench working on Centos 6.6: 1. Install the EPEL repository if you haven't already done so. This is an add on repository that provides a number of packages that are not included in the normal RHEL/Centos distributions. From a terminal prompt, switch to SU mode and enter this command for 32-bit installations:

###### rpm -Uvh http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm

or for 64-bit installations:

###### rpm -Uvh http://download.fedoraproject.org/pub/epel/6/x86\_64/epel-release-6-8.noarch.rpm

Update YUM by entering this command:

###### yum update

2\. Next, a couple of dependencies for MySQL Workbench need to be installed. From the command line enter the following commands, and when prompted answer yes:

###### yum install libzip-0.9-3.1.el6

###### yum install tinyxml

4\. Add the MySQL software repository by visiting the [MySQL web site](http://dev.mysql.com/downloads/repo/yum/) and choosing the link appropriate for your distribution. You'll be prompted to login (skip it by scrolling down to  the bottom of the screen and clicking the No thanks link). In the window that appears, choose the Open with Package Installer option, and click OK.  When asked if you want to install the file, click the Install button. After a few minutes you'll be prompted for your administrator password and the repo and dependencies will be added to your system. 5. Back at the command prompt run the following to install the application:

###### yum install mysql-workbench-community

Answer yes when prompted. After a few moments, workbench will be installed, and a shortcut will appear under the Applications menu - Programming submenu. Click on it to start the application. [![logo-mysql](http://edpflager.com/wp-content/uploads/2014/11/logo-mysql.png)](http://www.mysql.com/)