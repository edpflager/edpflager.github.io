---
title: Docker Admin Cheat Sheet part 1
tags: []
id: '3273'
categories:
  - - Blog
  - - Docker
  - - Linux
comments: false
date: 2016-04-25 19:23:51
---

##### [![container-lock](http://edpflager.com/wp-content/uploads/2016/04/container-lock-300x201.jpg)](http://edpflager.com/?attachment_id=3275#main)Here is part one of a personal docker administration cheat sheet I have been putting together. I know there are a number of sites that provide similar tools, but for my own purposes, its easier to remember different commands when I organize them myself.

# Docker administration cheat sheet

_Docker images are a source file (like an .ISO) that is used to start a container (an installed system). Periodic maintenance is necessary because a Docker container remains on your system even after it exits._

## What’s running?

Show all running local containers with container id, image it’s based on, any open ports, commands that it runs on startup, and when it was created.

*   Docker ps
    

Show all local containers (running or not) with short container id, image it’s based on, any open ports, commands that run on startup, and when it was created

*   Docker ps –a or -l
    

Show only the container id of all local containers (running or not running).

*   Docker ps -aq or -lq
    

Show the same info as –a or –l but with the full container ID rather than the shortened one.

*   Docker ps –a –no-trunc
    

Pause the container with the specified ID (which you can get with the ps command)

*   Docker pause <container id>
    

Restart a previously paused container with the specified ID (which you can get with the ps command)

*   Docker start <container id>
    

## Local Images

Show all locally stored image files, with a common name (referred to as a repository), a 12 character ID, the size of the image file and how long ago the image was created. There may also be a tag indicating the image file version

*   Docker images
    

## Shutdown containers

Gracefully shut down an active container with the id or short name (which you can get with the ps command) – this is preferable to kill.

*   Docker stop <container ID> or <name>
    

If stop doesn't work you can forcefully shut down an active container with the id or short name (which you can get with the ps command)

*   Docker kill <container ID> or <name>
    

To force a shut down of a running container and delete it

*   Docker rm –f <containerID>
    

## Cleanup

Delete the container identified with the specified ID (which you can get with the ps command)

*   Docker rm <container id>
    

Delete a docker image file (a source file for containers) identified with the specified id (which you can get with the ps command)

*   Docker rmi <image name>
    

To delete all of the containers on your local system (be careful!), use this command

*   Docker rm $(docker ps –a –q)
    

That's all for now! Coming up I have posts on running containers, and how to attach the host file system as a volume in a container.