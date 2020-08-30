---
title: Ubuntu 12.10 - Add PPA repositories
tags: []
id: '1236'
categories:
  - - Blog
  - - Linux
comments: false
date: 2013-04-14 13:59:18
---

[![package](http://edpflager.com/wp-content/uploads/2013/04/package.jpg)](http://edpflager.com/wp-content/uploads/2013/04/package.jpg)If you are using Ubuntu, you are probably familiar with the concept of repositories - Internet software archives where additional software packages are available to be installed. There are official repositories, supported by Canonical but other vendors and publishers also provide software in their own repositories. Configuration information for accessing these archives is stored in a file at: /etc/apt/sources.list. To add additional repositories to this file, and be able to install software from them,
<!-- more -->
depends on how the repository information is provided. If the publisher provides one or  several lines starting with "deb", then you can edit the sources.list file using your favorite text editor and add the information directly. Often though, you will see lines such as this: ppa:webupd8team/java. Add this information to your repositories list by using the command: sudo add-apt-repository ppa:webupd8team/java If you get a message that the command is not known, you will need to install a support package to get it to work. For versions of Ubuntu before 12.10, enter this command:

*   sudo apt-get install python-software-properties

For Ubuntu 12.10 and after the command was relocated to another package. You can add it this way:

*   sudo apt-get install software-properties-common

Once you have the ad-apt-repository command installed, you can add the ppa information to you sources.list file by executing the: sudo add-apt-repository ppa:<repository> and then execute: sudo apt-get update to update the system repository lists.