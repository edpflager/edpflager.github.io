---
title: Cannot connect to the Docker daemon - fixed!
tags:
  - guides
  - How-to
  - howto
  - Mint
  - SysAdmin
  - technical
  - Ubuntu
id: '3256'
categories:
  - - Blog
  - - Docker
  - - Linux
comments: false
date: 2016-02-06 19:10:08
---

[![containers](http://edpflager.com/wp-content/uploads/2016/02/containers-1529075-638x480-300x226.jpg)](http://edpflager.com/?attachment_id=3258#main)A quick note this time around: After installing Docker on my Linux Mint laptop, I wanted to be able to run it with my normal user account and not have to SUDO every time I wanted to start a container. (Please be advised that doing so can be considered a HUGE security hole, but since I am just testing Docker, I was willing to risk it). In order to do this, all of the documentation I found said to create a Docker group, and then add my user account to that group, and restart. This is fairly simple to accomplish with the GUI Users and Groups tool. The Docker daemon then uses that group to see if the user account has permissions to start Docker and connect to the daemon. The result? I would get this error: _Cannot connect to the Docker daemon. Is 'docker daemon' running on this host?_ After trying several things, I finally found the solution.

1.  From a terminal prompt run this command:
    
    echo $DOCKER\_HOST
    
    You should see a line indicating your Docker host is set to an IP address. You will need to clear it.
2.  At the terminal prompt, switch  to your home folder:
    
    cd ~
    
3.  Now, edit your shell profile:
    
     nano ./.profile
    
4.  Look for a line like this:
    
     export DOCKER\_HOST=tcp://127.0.0.1:4243
    
5.  Add a comment marker (#) before it or delete the line completely. Save your profile file and exit.
6.  Restart your system, and you should now be able to run Docker using your normal user account.