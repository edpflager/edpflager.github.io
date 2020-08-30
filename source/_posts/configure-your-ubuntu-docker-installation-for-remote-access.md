---
title: Configure your Ubuntu Docker installation for remote access
tags:
  - cookbook
  - guides
  - How-to
  - howto
  - install
  - SysAdmin
  - technical
  - Ubuntu
id: '3347'
categories:
  - - Blog
  - - Docker
  - - Linux
comments: false
date: 2016-07-03 08:16:36
---

[![container](http://edpflager.com/wp-content/uploads/2016/07/container-300x185.jpg)](http://edpflager.com/?attachment_id=3359#main)The default installation for Docker on Ubuntu server (16.04) configures the daemon (service) to listen on a local socket. But what if you want to access your Docker daemon remotely, from another box, If you are using the default configuration, you would need to open a Secure Shell to the server, and access Docker that way. But there is a way to setup Docker to allow remote access. First, lets verify that Docker is only working with a local socket.  On the server,  run from this command line:

ls -l /run

The results show have an entry with the docker.sock entry like this: [![dockersocket](http://edpflager.com/wp-content/uploads/2016/07/dockersocket-1024x178.png)](http://edpflager.com/?attachment_id=3348#main) And you can check that the daemon is not listening on any ports on your system by running this command:

sudo netstat -tlp

No Docker application should be listed in the results. [![NoDocker_port](http://edpflager.com/wp-content/uploads/2016/07/NoDocker_port-1024x107.png)](http://edpflager.com/?attachment_id=3355#main)
<!-- more -->
 

1.  Now change to **/etc/default**.
2.  As root, open the docker file in the folder with a text editor:
    
    nano ./docker
    
3.  Find the line that starts **#DOCKER\_OPTS=** and add a new line below it.
4.  The new line should be:
    
     DOCKER\_OPTS="-H tcp://192.168.0.22:2375"
    
    substitute your server's IP address for 192.168.0.22. The port specified **:2375** is the conventional one used for unencrypted communications to the docker daemon. Use **:2376** for an encrypted port.
5.  Save the file and exit.
6.  Restart your Docker service:
    
     sudo service docker restart
    
7.  Verify that Docker is now listening on the IP address specified, by rerunning this command:
    
     sudo netstat -tlp
    
8.  The results should show that Docker is indeed listening on the specified port[![docker_port](http://edpflager.com/wp-content/uploads/2016/07/docker_port-1024x130.png)](http://edpflager.com/?attachment_id=3351#main)
9.  Docker is now setup to listen for remote communications. To test it, form your Docker server, run this command, again substituting your server's IP address:
    
    docker -H tcp://192.168.0.22:2375 version
    
10.  You should see results similar to this: [![dockerversion](http://edpflager.com/wp-content/uploads/2016/07/dockerversion-300x185.png)](http://edpflager.com/?attachment_id=3367#main)
11.  Docker is now responding to requests on the address and port you specified! When accessing your Docker machine remotely, be careful of mixing release candidate clients with stable Docker daemons. They don't play nice together.