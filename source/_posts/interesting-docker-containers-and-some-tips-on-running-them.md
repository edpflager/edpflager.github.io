---
title: Interesting Docker Containers (and some tips on running them)
tags:
  - CDH
  - Cloudera
  - Hadoop
  - How-to
  - howto
  - install
  - Linux
  - technical
id: '3376'
categories:
  - - Big Data
  - - Blog
  - - Business Intelligence
  - - Docker
comments: false
date: 2016-07-31 13:10:13
---

[![shipping-82339](http://edpflager.com/wp-content/uploads/2016/07/shipping-82339-300x225.jpg)](http://edpflager.com/?attachment_id=3385#main)I've been learning how to use Docker for the last couple of months. Part of that experience has been downloading and working with various freely available containers from the Docker Hub. Since an ever increasing number applications are web based (i.e using a website as a UI tool) porting many open source projects to use Docker is becoming less complex. While not all open source applications can benefit yet from containerization, I think its only a matter of time before this technology starts to really gain in popularity.

My area of interest tends to fall more towards Business Intelligence and related technologies, so what I've been experimenting with are containers in that realm. This time around, I'll discuss these containers and my experience getting them running on a native Docker installation on Ubuntu:

*   Rstudio
*   Cloudera Hadoop
*   ODOO with Postgresql
<!-- more -->
##### RStudio

My latest experimenting has been with RStudio which is an IDE that allows you to execute code with the data analytics language R. To pull the image from the Docker Hub execute:

docker pull rocker/rstudio

Running the container with the command given in the documentation will assign an IP address from within the Docker created subnet. On my system this caused a significant delay and then the browser on my client machine timed out. To assign the Host server's IP address and make the container available without setting up additional routing on my client I needed to modify the provided command and insert the Docker host's IP address into the command like this:

docker run -d -p <host IP address>:8787:8787 rocker/rstudio

Accessing the address: **<host IP address>:8787** brings up the RStudio interface quickly and you can login with the default UserID and password: rstudio/rstudio

#####  CLOUDERA HADOOP

Cloudera - developers of one of the most popular Hadoop distributions, offers a quick start Docker version of their Hadoop platform on their [website](http://www.cloudera.com/downloads/quickstart_vms/5-7.html). Since my server doesn't have a GUI and the Cloudera website doesn't provide a link (that I could use **wget** with) unless you provide all of your information to them, I opted to pull a container from the Docker hub with:

pull cloudera/quickstart:latest

Either way, once you have it downloaded, you can start the container with a series of options. For ease of use, I dropped the command line into a BASH script and just execute that to start it :

docker run --hostname\=quickstart.cloudera --privileged\=true -t -i -p 8888:8888 cloudera/quickstart /usr/bin/docker-quickstart

Switching over to my client, in a web browser, I can access **<host IP address>:8888** to bring up the Cloudera HUE interface and login with the default User ID and password: cloudera/cloudera

##### ODOO WITH POSTGRESQL

ODOO or OpenERP as it was called initially, is a huge suite of business applications that can be installed to meet most needs for small to very large enterprises (ODOO says on their website that Toyota uses their software for some of its needs). They pride themselves on ease of use, which in my experience tends to be accurate. The Community Edition lacks some of the features in the paid edition, but for a smaller company, or one testing ODOO out, it should be sufficient for most needs (no support however).

The ODOO container is available on the Docker Hub and can be pulled with:

 pull odoo

Odoo requires a PostgreSQL 9.4 database server as a backend, and their online documentation provides a command line that sets up the PostgreSQL container with the expected container name and two environment variables that Odoo will be attempting to use:

docker run -d -e POSTGRES\_USER=odoo -e POSTGRES\_PASSWORD=odoo --name db postgres:9.4

Once you have the database server running, you can run the ODOO container with a fairly straightforward Docker command that maps the container port to 8069, and links to the PostgreSQL container with the --link switch:

docker run -p 8069:8069 --name odoo --link db:db -t odoo

This command uses a -t switch to show you on the terminal what is happening within the ODOO container.  For my purposes, I would prefer not to show that, but instead would use this command to run ODOO as a daemon:

docker run -d -p 8069:8069 --name odoo --link db:db odoo 

If I need to see the screen responses, I can use the Docker logs command with any switches necessary to trim down the output. To see whats happened on screen since the container was started up for example:

docker logs odoo

Switching over to my client, in a web browser, I can access **<host IP address>:8069** to bring up the ODOO database creation website. Enter in the requested info and click CREATE DATABASE. After a few minutes, the database will be created, and the webpage will change and prompt you to login using the credentials you supplied in the first screen. Login and you are ready to start configuring and using ODOO. One thing to watch for with ODOO: If you shutdown the ODOO container, be sure to restart the PostgreSQL container before restarting ODOO.

The best part of this process is I can have all three (four if you count the PostgreSQL one) up and running at the same time. While it was possible to do this previously using VMs there is considerably less overhead on my server running Docker with these containers. Hopefully that illustrates why I am excited about Docker and what it brings to the open source landscape.