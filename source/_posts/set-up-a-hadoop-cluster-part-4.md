---
title: Set up a Hadoop Cluster - Part 4
tags:
  - CDH
  - Hadoop
  - HortonWorks
id: '1746'
categories:
  - - Big Data
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2014-02-01 12:21:54
---

[![apache-ambari-project](http://edpflager.com/wp-content/uploads/2014/02/apache-ambari-project.jpg)](http://edpflager.com/wp-content/uploads/2014/02/apache-ambari-project.jpg)This is part 4 of my series about setting up an inexpensive Hadoop cluster using refurbished desktop PCs as Linux servers. (see here for parts [1](http://edpflager.com/?p=1714 "Set up a Hadoop Cluster – Part 1"), [2](http://edpflager.com/?p=1721 "Set Up a Hadoop Cluster – Part 2") and [3](http://edpflager.com/?p=1725 "Set up a Hadoop Cluster – Part 3")). I installed HortonWorks HDP 2.0. As part of the installation, the main server also installs [Ambari](http://ambari.apache.org/) - an open source server application that is used to monitor and manage Hadoop clusters (hence my reason for adding a fourth server with more memory than the other three) and Nagios - a server monitoring application.
<!-- more -->
The installation is pretty easy. Download the management application package and start it. You provide the server names or the IP addresses of the various servers, and the private SSH key that you set up earlier. Ambari attempts to connect to each of the servers, and installs the various components from the Hadoop ecosystem across your setup. It attempts to balance which components go on each server, providing redundancy and allows you to override the default configuration.

#### **Note: As part of the installation be sure to turn OFF the iptables service on each of the servers before you start and turn ON the NTP service. If your cluster will be for test purposes, you are probably OK with leaving iptables disabled. The NTP service on each of your nodes allows them to sync date and time across your cluster, which is very important. If you don't have this service enabled, you'll likely get an error in your Cloudera Manager console.** 

Once it was complete, I attempted to start the services via Ambari. This part is not so intuitive, and I struggled here. Finally I found the Start All/Stop All buttons and used them. Unfortunately, some services would start OK and after a few minutes would shut down. Very frustrating! After struggling with it for a couple of hours, the only thing I could determine was possibly the servers needed more memory. The first server in the cluster had the bulk of the components installed on it (twenty), but only 2GB of RAM. Rather than go down that path, I have decided to remove HortonWorks and try Cloudera.