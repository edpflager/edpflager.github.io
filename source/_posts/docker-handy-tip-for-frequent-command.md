---
title: Docker - Handy Tip for Frequent Command
tags:
  - cookbook
  - guides
  - How-to
  - howto
  - SysAdmin
  - technical
id: '3371'
categories:
  - - Blog
  - - Docker
  - - Linux
comments: false
date: 2016-07-09 19:42:50
---

[![containers-small](http://edpflager.com/wp-content/uploads/2016/07/containers-small-300x225.jpg)](http://edpflager.com/?attachment_id=3372#main)One command I use a lot when working with Docker, is **docker ps -a** to see what containers I have and which ones are active. To save a little time when entering that command, create an alias in your BASH profile for that command.

1.  From a command prompt, change to your home folder: cd ~
2.  Edit the .bashrc file with a text editor. In my case, I use NANO: nano ./.bashrc (Note: Earlier versions of Mint supported editing the .profile file. This no longer works).
3.  Add this line to the end of the file:  **alias dpa="docker ps -a"**
4.  Save the file with CTRL-O and exit back to a command prompt with CTRL-X.
5.  Logout and log back in.
6.  At a command prompt enter: **dpa**. You'll see the results of **docker ps -a** but without all the typing!