---
title: Run Pentaho BIServer on Linux - Demystified!
tags:
  - Big Data
  - howto
  - install
  - technical
id: '2066'
categories:
  - - Blog
  - - Business Intelligence
  - - Pentaho
comments: false
date: 2014-05-08 03:44:32
---

[![frustration](http://edpflager.com/wp-content/uploads/2014/05/frustration-300x224.jpg)](http://edpflager.com/wp-content/uploads/2014/05/frustration.jpg)I'm posting this just because there seems to be a lot of confusion and misinformation on the web about getting the open source version of [Pentaho's BIServer](http://community.pentaho.com/) up and running on Linux. (I know I spent several hours over the course of a week running through them.) The current version is 5.01 and you download a ZIP file that contains almost everything you need to get started. You do NOT need to install MySQL or PostgreSQL or Oracle on your server to get Pentaho BIServer to work, nor do you need to set the PENTAHO\_JAVA\_HOME or JAVA\_HOME system variables or create a Pentaho user account. If you want to change the repository database to something besides the embedded HSQLDB you can find directions on that on the Internet (Good luck to you). If you just want to setup a simple system for testing and low volume usage, read on...
<!-- more -->
Here is a step by step account of how I got Pentaho BIServer to run on my system:

1.  Install a Linux server ( I am using CentOS 64-bit). A minimum of 2GB of RAM is necessary, although 4GB or more is better.
2.  Remove any unneeded applications and install any system updates.
3.  Disable iptables and iptablesv6 (be cautious of this if you plan to use this in a production environment!)
4.  Edit [/etc/sysconfig/selinux](http://edpflager.com/?p=1866 "Disable SELinux to install Cloudera") (RedHat based Linux distros) and set it to disabled. Restart your server for all the config changes to take effect.
5.  If its present, remove OpenJDK software (**java -version** will tell you).
6.  Download Oracle's JAVA 1.7\_55 RPM package from their [website](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) and install it.
7.  All of this above was the difficult part. :)
8.  Download the Pentaho biserver-ce from [Pentaho's website](http://community.pentaho.com) and click the Download Pentaho CE link. You be redirected to a page on SourceForge.net and the file will begin downloading. It takes a while.
9.  Once the download completes, create a pentaho folder in your home folder and extract the biserver-ce-5.0.1-stable.zip to it.
10.  At the command line, switch back to your user account if you are still running as root.
11.  Navigate to your /home/username/pentaho/biserver-ce folder.
12.  To start the server, enter this command: ./start-pentaho.sh
13.  The shell script will produce several lines of output, and then return to a command prompt.
14.  If you are running a GUI environment on your server, open a browser, and navigate to http://localhost:8080/pentaho. Otherwise open a browser on a machine on the same network and access the IPAddress for your server with the ending notation (:8080/pentaho).
15.  It may take a few minutes for the webpage to load up, but once it does, you'll see the Pentaho User Console login screen. Login using the default user account. User name: admin     Password: password
16.  You're in!

If you would like to access the server from a different machine, set a static IP address and a fully qualified domain name, but if all of your work will be local, you are all set.