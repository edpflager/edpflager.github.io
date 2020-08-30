---
title: Set up a Hadoop Cluster - Part 3
tags:
  - CDH
  - Hadoop
  - HortonWorks
id: '1725'
categories:
  - - Big Data
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2014-01-28 09:23:24
---

[![services](http://edpflager.com/wp-content/uploads/2014/01/services-300x160.png)](http://edpflager.com/wp-content/uploads/2014/01/services.png)[Third part of this series](http://edpflager.com/?p=1721 "Set Up a Hadoop Cluster – Part 2"), and we haven't even gotten to installing Hadoop yet! This is a quick one though. One of the things you need to make Hadoop work is to be able to use SSH across all of the servers in your cluster. To enable it, log into each box, and go to System in the menu, Administration from the menu and over to Services. Click on it.
<!-- more -->
The Services window will open. In the left pane, scroll down until you find "sshd". If it has a red dot next to it, it means the ssh daemon is not running and you won't be able to connect to the box from your main Hadoop server. Click on the line, and then click on the Enable button at the top of the window. After a few seconds, the dot next to "sshd" will change to green. You're almost there! Now up in the top again, click on the button labeled Start. Underneather the Stop and Restart buttons is a small panel that will show you the status of the SSH daemon. It should now say the service is enabled and the service is running. If it does, you are good to go! Now you just need to enable password less SSH access to each of the boxes from your main one. For a good walkthrough of setting this up, [look here](http://www.thegeekstuff.com/2008/11/3-steps-to-perform-ssh-login-without-password-using-ssh-keygen-ssh-copy-id/)