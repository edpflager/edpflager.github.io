---
title: Services GUI tool on CentOS
tags:
  - SysAdmin
id: '1871'
categories:
  - - Blog
  - - Linux
comments: false
date: 2014-03-12 17:29:22
---

[![services](http://edpflager.com/wp-content/uploads/2014/03/services-300x221.png)](http://edpflager.com/wp-content/uploads/2014/03/services.png)SysAdm purists often look down on people who use a GUI to handle tasks on  their servers, but having worked for several years on Novell Netware at the beginning of my career (shudder), give me a GUI over a command line every time! On my home CentOS servers, I have the GNOME desktop environment loaded, and it makes me a lot more productive, because I don't have to remember the locations of many scripts, or the various command line switches to run various applications. Recently, I was installing a replacement server for my Hadoop cluster, and I found that the Services GUI option was not present under the System - Administration menu. A little hunting turned up that the system-config-services package wasn't installed. If this happens to you, here's a quick way to get it back. Open up a terminal - kidding!!!

1.  Start the Add/Remove Software application from the Administration menu under System.
2.  Search for system-config-services and check the two options that should appear. One is the application, the other the documentation.
3.  Click Apply down in the lower right corner, and authenticate as Root.
4.  Wait a few second, then check under System - Administration. Service should be back right above Software Update.