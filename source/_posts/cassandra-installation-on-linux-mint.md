---
title: Cassandra Installation on Linux Mint
tags:
  - cookbook
  - guides
  - How-to
  - howto
  - Mint
  - SysAdmin
  - technical
  - Ubuntu
id: '3168'
categories:
  - - Big Data
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2015-12-20 18:46:18
---

[![Cassandra_logo](http://edpflager.com/wp-content/uploads/2015/12/Cassandra_logo.png)](http://edpflager.com/?attachment_id=3216#main)NoSQL databases are multiplying faster that anyone could have imagined when Google released their Google File System and MapReduce papers in 2003 and 2004 - the inspiration for a host of NoSQL database developers. The NoSQL project that became Apache Cassandra was originally developed at Facebook as the back-end for their Inbox Search feature. It was released as open source in July 2008 and later moved to the Apache Foundation. Gaining in popularity in the intervening time,  the website [db-engines.com](http://db-engines.com/en/ranking) now ranks Cassandra the eighth most popular data management system. Setting up a development version of Cassandra on Linux Mint is pretty straightforward, and this tutorial will walk through the process. At the time of this writing, Cassandra 3.0.1 had just been released. It requires Java 8 or later and at least 8GB of RAM. Version 2.2.4 is available as the most current stable version, which will run with Java 6 or later with 4GB or RAM. If you intend to use [DataStax's OpsCenter](https://docs.datastax.com/en/upgrade/doc/upgrade/opscenter/opscCompatibility.html) management tool be aware that it does not currently support either version. To illustrate the installation of OpsCenter I will be covering the older Cassandra version 2.1 which IS supported by OpsCenter. DataStax notes on their website that the next version of OpsCenter will support Cassandra 2.2 and 3.0. The difference between installing version 2.2 or 3.0 is minimal and I'll note that as we process. Also, be aware that the Linux Mint update center may try to upgrade your system to 3.0 once you are complete.
<!-- more -->
##### Cassandra Installation

Before starting, make sure your Linux Mint system is configured properly. See my previous post about [setting up a Linux Mint server.](http://edpflager.com/?p=3173) Once that is complete, continue with the instructions below.

1\. Edit the source list file to add the Cassandra repository information. In a terminal:

sudo nano /etc/apt/sources.list

2\. On Linux Mint, this file is likely to only have one line in it. Go to the bottom of the file and add these two lines:

deb http://www.apache.org/dist/cassandra/debian 21x main
deb-src http://www.apache.org/dist/cassandra/debian 21x main

3\. To install the newer 3.0.1 version , change the 21x to 30x or if you want to install version 2.2.4 change it to 22x.

4\. Now, we need to add the public keys to authenticate the Cassandra repository:

gpg --keyserver pgp.mit.edu --recv-keys F758CE318D77295D
gpg --export --armor F758CE318D77295D | sudo apt-key add -

gpg --keyserver pgp.mit.edu --recv-keys 2B5C1B00
gpg --export --armor 2B5C1B00 | sudo apt-key add -

gpg --keyserver pgp.mit.edu --recv-keys 0353B12C
gpg --export --armor 0353B12C | sudo apt-key add -

5\. Update your system's cached repository information with:

sudo apt-get update

6\. Now enter  this command to install Cassandra:

sudo apt-get install cassandra

7\. Any dependent packages will be identified and you'll be prompted to install all of them, along with Cassandra. Touch Y to do so. Once installation completes, you'll be returned to the terminal prompt. At this point you may want to restart the server, just to make sure all is well with your setup.

8\. From a command prompt, start Cassandra with the command:

sudo cassandra

9.After a few seconds, your terminal window will fill with a large amount of text as Cassandra starts up. Once it completes,  and you are returned to a command prompt (or if it appears to hang, touch ENTER), you can access the interactive shell for Cassandra by entering this:

cqlsh

10\. The cqlsh shell (Cassandra SQL Shell)  will display its version number and the version of your Cassandra installation and then return to a > prompt for input of commands.

[![cqlsh](http://edpflager.com/wp-content/uploads/2015/12/cqlsh-300x48.png)](http://edpflager.com/?attachment_id=3212#main)

11\. Type "quit" (without the quotes) at the prompt and you'll exit the cqlsh application.

 

##### DataStax OpsCenter Installation

1\. Edit the source list file to add the Cassandra repository information. In a terminal:

1.  sudo nano /etc/apt/sources.list
    
    2\. Go to the bottom of the file and add this line:
    
    deb http://debian.datastax.com/community stable main
    
    3\. Download the Add the DataStax repository key:
2.  $ sudo wget https://debian.datastax.com/debian/repo\_key
    

4\. Install the key you just downloaded with:

sudo apt-key add ./repo\_key

5\. Install the DataStax OpsCenter package using the APT Package Manager:

$ sudo apt-get update
$ sudo apt-get install opscenter

6\. Start the OpsCenter software with the command:

sudo service opscenterd start

7\. In order for the OpsCenter software to communicate with your Cassandra installation, you'll need to install the Datastax agent:

sudo apt-get install datastax-agent

8\. You'll need to make one configuration change to the DataStax **address.yaml** file.

sudo nano /var/lib/datastax-agent/conf/address.yaml 

9\. The file is probably empty at this point. Add this line to it, putting your IP address in place of the <phrase>:

stomp\_interface: <ip\_address of the server>

10. Now from a web browser access your Cassandra system on port 8888, i.e.: http://cassandra.yourdomain.com:8888. If all has gone well, OpsCenter will load and you'll see the dashboard indicating your systems health.

Cassandra logo By Apache Software Foundation used under Apache License 2.0 (http://www.apache.org/licenses/LICENSE-2.0)\], via Wikimedia Commons