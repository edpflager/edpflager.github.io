---
title: Set up a Hadoop Cluster – Part 5
tags:
  - CDH
  - Hadoop
id: '1769'
categories:
  - - Big Data
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2014-02-07 15:15:15
---

[![ClouderaManager](http://edpflager.com/wp-content/uploads/2014/02/ClouderaManager-300x183.png)](http://edpflager.com/wp-content/uploads/2014/02/ClouderaManager.png)In [Part 4](http://edpflager.com/?p=1746 "Set up a Hadoop Cluster – Part 4") of this series, I talked about how I installed HortonWorks' Hadoop distribution on my small test cluster, and how I had some problems with the various services shutting down repeatedly. My initial feelings were that the server with Ambari and Nagios (along with 18 other services) possibly needed more memory, but because the Zotac ZBox I was using would only support 4GB, I was kind of stuck. I could have added more memory to the other boxes in the cluster (which have 2GB each), and reallocated the services, but I wanted to get a vanilla installation up and running reliably before I started tweaking it.
<!-- more -->
After some consideration I rebuilt the cluster, reinstalling the OS and patches, removing unwanted software and services. I could have [uninstalled](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.0.2/bk_installing_manually_book/content/rpm-chap15.html) HortonWorks using the directions from their website, but to ensure I had a clean setup to begin with, I opted for the rebuild. Once that was complete, I followed [Cloudera's instructions](http://www.cloudera.com/content/cloudera-content/cloudera-docs/CM4Ent/latest/Cloudera-Manager-Installation-Guide/Cloudera-Manager-Installation-Guide.html) for installing their CDH4 management tool, and then proceeded to install the software components for the cluster on the various boxes. As with Hortonworks distribution, the install was pretty straightforward, and pretty painless, just make sure you have iptables turned off, and also make sure SELinux is disabled on your boxes too. One nice feature was the ability to select different installation scenarios that included bundles of various Hadoop components. Unlike Hortonworks, which bundles Nagios in for network monitoring, Cloudera integrates their own monitoring tools into the management package. It seems to me to be a little more intuitive than HortonWorks, but that is a judgement call. If you are familiar with Nagios, then its probably a minimal difference. Cloudera uses two web interfaces - one for administration of your cluster (CDH Manager) and one for using the various components (HUE). The CDH Manager web interface starts up when your cluster starts up, but you still have to login to it and start the management service, and then start the various components. You can start them individually, or all of them as a whole (recommended). Either way, you'll see the progress of the start up graphically, and if any issues occur, just click on the service to find out more details. Everything was working right out of the box, except HBASE and Cloudera. I received error messages about the Thrift service not running. How that was resolved will be discussed in an upcoming entry. Other than that, my cluster was up and running!