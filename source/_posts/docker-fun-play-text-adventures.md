---
title: Docker Fun - Play Text Adventures
tags:
  - guides
  - How-to
  - howto
  - humor
  - SysAdmin
  - technical
  - Ubuntu
id: '3412'
categories:
  - - Blog
  - - Docker
  - - Linux
comments: false
date: 2016-10-15 15:57:50
---

[![zork](http://edpflager.com/wp-content/uploads/2016/10/zork-300x165.png)](http://edpflager.com/?attachment_id=3414#main)This time around in the Docker Fun series, we are getting a little more ambitious. Zork was one of the first popular text adventures, building on the work of Will Crowther and Dan Wells who created the mainframe game Colossal Cave (aka Adventure). Zork was originally written to run on a DEC PDP-10 system, and was later ported to just about every personal computer that was available. While text adventures at that time were struggling with two word commands like "Open Door", Zork understood much more elaborate command like "Hit the grue with the Elvish sword". The developers of Zork founded Infocom and eventually released a number of sequels and prequels to Zork as well as several dozen other text adventures in a number of different genres. Eventually the company was sold to Activision and the Infocom games have since lapsed into a sort of purgatory. Technically the download file for Zork is in violation of copyright laws, so feel free to substitute one of the many free Z5 files available on the Internet.
<!-- more -->
1.  On your Docker host machine, open a terminal prompt and create a new folder called "xyzzy" (for interactive fiction).
2.  Within that folder create a new Dockerfile text file. Enter the following text in the file:
    
    FROM ubuntu:14.04
    MAINTAINER noone
    RUN apt-get update && apt-get -y install frotz && useradd player 
    WORKDIR /data
    
3.  Save the file.
4.  In the xyzzy folder, enter the following command to build your container (make sure to include that final period):
    
    docker build -t xyzzy .
    
5.  Several dozen lines will scroll by as the container is built, and eventually you should see a Successfully built message.
6.  Because FROTZ won't run as ROOT, we include code to add a FROTZ user (player) in the Dockerfile. Then when the container is run you need to specify to use the FROTZ account rather than ROOT.
7.  Finally we want to have our game file and any save files be persistent once the container ends, so we will map a folder on the host to the container. Create a folder in your user folder called data1 and save the z5 file there.
8.  Run your new container with this command: docker run -it -u player -v /home/<username>/data1:/data xyzzy /usr/games/frotz /data/zork\_1.z5
9.  Lets dissect that command for a second:
    *   \-it is for an interactive container - one we can work with.
    *   \-u player is so we can run the container as user player.
    *   \-v /home/<username>/data1:data maps the host system user's data1 folder to the data folder in the container.
    *   xyzzy is the name of the container
    *   /usr/games/frotz is the application we want to run in the container
    *   /data/zork\_1.z5 is the data file we want the Frotz application to use.
10.  The container will open and display a screen similar to the one above. To exit just hold CTRL-C on your keyboard. Save and Restore work because the working directory is changed to the data1 folder on the host.