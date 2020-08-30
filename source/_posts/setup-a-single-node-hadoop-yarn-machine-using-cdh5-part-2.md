---
title: Setup a Single-node Hadoop Yarn machine using CDH5 - Part 2
tags:
  - CDH
  - Cloudera
  - Hadoop
  - How-to
  - howto
  - install
  - SysAdmin
  - technical
  - YARN
id: '2490'
categories:
  - - Big Data
  - - Blog
  - - Linux
comments: false
date: 2014-10-08 14:08:26
---

[![hbase_zoo](http://edpflager.com/wp-content/uploads/2014/04/hbase_zoo-300x170.png)](http://edpflager.com/wp-content/uploads/2014/04/hbase_zoo.png)This is part 2 of setting up a single-node Hadoop Yarn system for sandbox use. Part 1 was [here](http://edpflager.com/?p=2475 "Setup a Single-node Hadoop Yarn machine using CDH5 – Part 1"), or for the series for using MapReduceV1, go here. I'm hoping to keep this series in a similar order as the original set of articles, and will deviate only when necessary. All the content here is based on the [Cloudera](http://www.cloudera.com) documentation, but I’ve modified it to be easier to follow for setting up a pseudo cluster and added additional content where necessary. Please be careful when copying lines from these articles to paste into Hadoop config files or a terminal window. I have found that the double hyphen characters used in the comment lines may copy over as a long hyphen instead. This is likely to cause issues when attempting to run the various components.

##### Install HBase
<!-- more -->
1.  Open a terminal window on the CentOS server and switch to the root user account with:  **su -**
2.  Enter this command: **yum install hbase** 
3.  Type Y when prompted, and hit enter. After a few seconds the Hbase package will be downloaded and installed.
4.  HBase can use a large number of files at once which may conflict with a system configuration called ulimit that allows a default maximum of 1024 concurrent open files. To increase this limit, as the root user in the terminal window, edit the **/etc/security/limits.conf** file.
5.  At the bottom of the file, before the #End of file line, add these two lines:  
          **     hdfs             –       nofile  32768**  
     **hbase           –       nofile  32768**
6.  Restart the system to allow the changes to take effect.
7.  Edit the **/etc/hadoop/conf/hdfs-site.xml** file to allow the data node to serve a larger amount of files by adding the following property section to the file. (4096 is the minimum that should be used and xcievers is spelled correctly)  
            **<property>**  
     **<name>dfs.datanode.max.xcievers</name>**  
     **<value>4096</value>**  
     **</property>**
8.  Restart HDFS by either restarting the server, or use this command:  
          **for x in \`cd /etc/init.d ; ls hadoop-hdfs-\*\` ; do sudo service $x restart ; done**
9.  At this point HBase is installed in standalone mode, and it should be switched to a pseudo-clustered mode. Install the HBase Master which controls the HBase system,and several other servers. Once again at the command line as root, enter the following:  
     **yum install hbase-master**
10.  Once the install completes, start the service with this command:  
          **service hbase-master start**
11.  Open a web browser and point it to the system name and port 60010. For example [http://HadoopTest:60010](http://hadooptest:50070/) . In CDH version 5, you’ll see a website with multiple tabs at the top that you can click through to check out the status of the HBase system.
12.  Scroll down on the HOME page, and there should be one entry under the Region Servers section. Click on the Region Server link, and the webpage will refresh with the Region Server status information. At the bottom of the page is a link to return to the HBase Master webpage.
13.  Finally, in the terminal window, to access the HBase command line, enter the command: **    hbase shell.**
14.  Check the version number of HBase with the command: **version**
15.  Exit the HBase CLI with the command: **exit**

##### INSTALL A THRIFT SERVER

1.  Now install a Thrift server and a REST server to communicate with HBase. As root in a terminal window, enter this command: **yum install hbase-thrift** and then start it with: **           service hbase-thrift start**
2.  Install the REST server next. At the command line, enter: **yum install hbase-rest**
3.  The HBase REST server by default uses port 8080, which is a commonly used one. It can be left as is, or changed. To alter the port, edit the config file /etc/hbase/conf/hbase-site.xml and add this property section between the configuration tags, using another port in the value section:  
     **<property>**  
     **<name>hbase.rest.port</name>**  
     **<value>60050</value>**  
     **</property>**
4.  Still within the hbase-site.xml file, add these two additional properties:  
     **<property>**  
     **<name>hbase.cluster.distributed</name>**  
     **<value>true</value>**  
     **</property>**  
     **<property>**  
     **<name>hbase.rootdir</name>**  
     **<value>hdfs://localhost:8020/hbase</value>**   
     **</property>**
5.  **The value for hbase.rootdir's hostname must match the  hostname value in /etc/hadoop/conf/core-site.xml’s fs.default.name or fs.defaultFS property.**
6.  Save the file.
7.  Create the hbase directory in HDFS and give ownership to the HBase account, by entering these two lines (be sure to use the sudo – u hdfs portion to make sure the command is run by the correct Hadoop user):  
             **sudo -u hdfs hadoop fs -mkdir /hbase**  
             **sudo -u hdfs hadoop fs -chown hbase /hbase**

INSTALL ZOOKEEPER

1.  Before the HBase installation is complete, Zookeeper -Server needs to be installed and configured to run on a single server. Install the package by entering this command as root: **     yum install zookeeper-server**
2.  Create a Zookeeper folder in the local Linux file system and change the ownership by entering the following:  
             **mkdir -p /var/lib/zookeeper**  
            ** chown -R zookeeper /var/lib/zookeeper/**
3.  Since this is a first time installation of Zookeeper-server, initialize and start Zookeeper, by entering these commands as root:  
     **service zookeeper-server init service zookeeper-server start** (You’ll receive a message about specifying a myid if you are running in non-standalone mode. Its safe to ignore it).
4.  **Please Note**: For a production system Hadoop should be installed with an odd number of Zookeeper servers to maintain reliability. Because this is a pseudo-distributed mode installation primarily for testing and development, it is OK to use a single Zookeeper server.

##### START HBASE-MASTER

1.  Once the Zookeeper subsystem is running, start the HBase-Master service with this command: **     service hbase-master start**
2.  A couple of subordinate services for HBase need to be installed now. To install the region server, type the following command as root:  
     **yum install hbase-regionserver**
3.  And start it with: **       service hbase-regionserver start**
4.  At this point, nine services should be running as part of the Hadoop Yarn installation. To verify this, enter the following command in a terminal window as root: **jps**  A list of ten services with their PID should be returned. The tenth one is the Jps command.
5.  Open a web browser and point it to the system name and port 60010. For example [http://HadoopTest:60010](http://hadooptest:50070/) . Scroll down slightly to the Region Servers section on the HOME page, and there should now be two servers listed.

At this point, you have a basic Hadoop Yarn system configured with HBase and Zookeeper. You can access the various components using the command line tools. Next time, the configuration of the SNAPPY compression library and the first portion of installing and configuring HUE, Cloudera's web interface for working with a Hadoop cluster.