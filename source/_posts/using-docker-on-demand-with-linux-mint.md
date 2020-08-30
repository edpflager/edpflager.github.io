---
title: Using Docker on demand with Linux Mint
tags:
  - How-to
  - howto
  - Mint
  - SysAdmin
  - technical
  - Ubuntu
id: '3280'
categories:
  - - Blog
  - - Docker
  - - Linux
comments: false
date: 2016-05-21 16:33:12
---

[![container_shipping](http://edpflager.com/wp-content/uploads/2016/05/container_shipping-300x177.png)](http://edpflager.com/?attachment_id=3290#main)If you are like me and work on multiple things on your development system, you don't always want everything running when you start your PC. I've previously covered starting other services on demand, and this time around I'll cover running Docker as needed. Docker has essentially two separate components. There is the Docker daemon (or service) that is configured to start when the system is booted up and there is the Docker CLI that you interact with and your commands are passed to the daemon. The CLI only runs when you specifically call it from the terminal prompt with the DOCKER command. For my purposes, I didn't need or want the daemon running all the time because its a laptop. (If I was using a production system or even a full blown development box, I would prefer to have the daemon always running.) So after installing Docker, I needed to configure it to not start up every time the system starts, and then come up with an easy way to start it as needed.
<!-- more -->
##### Docker daemon configuration

1.  Open a terminal prompt and navigate to the **/etc/init** directory.
2.  Open the Nano editor as root:
    
    sudo nano ./docker.conf
    
3.  Near the top of the file, find the following line and comment it out by adding a couple of pound symbols (##) in front of it:
    
    start on (filesystem and net-device-up IFACE!=lo)
    
4.  Save the file and exit.
5.  At this point if you restart your PC, your Docker daemon shouldn't start up when your system does anymore. But we still need a way to start it on demand.

##### Desktop shortcuts  TO START AND STOP DOCKER

There are a number of ways to create a desktop shortcut to control the Docker daemon depending on your distro. You can also choose to just open a terminal prompt when you like and execute the start-up command and shutdown commands:

sudo service docker start  OR sudo service docker stop

[![launcher](http://edpflager.com/wp-content/uploads/2016/05/launcher-300x128.png)](http://edpflager.com/?attachment_id=3288#main)I prefer to add a desktop shortcut and to add the command to the menu system on Mint. In Mint 17.3, you can right click the desktop and click the option to **Create a new launcher here.** A Launcher Properties window will appear, like the one pictured here. Fill it in as I have. Before you click OK, you can assign your launcher a unique icon by clicking on the rocket button on the left. You'll be prompted to navigate to where the picture file is you would like to use is stored. Once you have selected it and returned to the Launcher properties, then click OK.

###### For my icons, I created PNG files using the [Docker whale icon](https://www.docker.com/brand-guidelines) and added a green up arrow and a red down arrow to the image. Due to Docker usage guidelines, I can't share those here.

You'll now receive a message asking if you want to add the item to the menu system as well, and informing you that initially it will be placed in an Other category. If you would like it added, click YES, otherwise click NO. If you would like to create an additional launcher to shutdown Docker when you don't need it, repeat the process above and change the name to Docker Down and the command to: **sudo service docker stop** That's it!