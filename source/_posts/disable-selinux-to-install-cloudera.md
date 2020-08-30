---
title: Disable SELinux to install Cloudera
tags:
  - CDH
  - Hadoop
id: '1866'
categories:
  - - Big Data
  - - Blog
  - - Linux
comments: false
date: 2014-03-09 17:05:48
---

[![SELinux](http://edpflager.com/wp-content/uploads/2014/03/SELinux-300x186.png)](http://edpflager.com/wp-content/uploads/2014/03/SELinux.png)One of the gotcha's I've encountered when installing Cloudera on CentOS Linux (and I'm assuming it holds true for RHEL), is the presence of SELinux. SELinux is a kernel module that supports some very strong access control security measures. While disabling this module won't make your system completely vulnerable to outside hacking of your systems, it does make it easier. If you are planning on using your Hadoop system exposed to the Internet in any way, you probably do not want to do what I am going to show you here. Or if you do, reenable SELinux once you are finished. Use at your own risk. To disable SELinux, open a terminal prompt and switch to Super User mode. Then in your favorite editor, open the file /etc/sysconfig/selinux. The file itself is very small, and you only need to make one change. About halfway down you will see a line like this: SELINUX=enforcing Change the word "enforcing" to "disabled", and save the file. Restart your box, and SELINUX will be disabled, and you can install Cloudera.