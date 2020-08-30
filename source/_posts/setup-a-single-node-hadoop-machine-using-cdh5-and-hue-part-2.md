---
title: Setup a Single-node Hadoop machine using CDH5 and HUE - Part 2
tags:
  - CDH
  - Cloudera
  - Hadoop
  - How-to
id: '1964'
categories:
  - - Big Data
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2014-04-16 15:12:01
---

[![hbase_zoo](http://edpflager.com/wp-content/uploads/2014/04/hbase_zoo-300x170.png)](http://edpflager.com/wp-content/uploads/2014/04/hbase_zoo.png)In [part 1 of this series](http://edpflager.com/?p=1945 "Setup a Single-node Hadoop machine using CDH5 and HUE – Part 1"), I walked through the beginning steps for setting up a single-node Hadoop pseudo-cluster. In that article, I showed you how to configure CentOS 6.5 with the necessary prerequisites for installation, download Cloudera's Hadoop distribution  (CDH5) and install it. In this article, I'll explain how to install:

*   HBase - an open source non-relational database that runs on top of the Hadoop File System (HDFS)
*   Zookeeper - an engine that provides distribution, synchronization and naming services for Hadoop,
*   and SNAPPY a fast data compression and decompression library that incorporates with many different components in the Hadoop ecosystem.

This series is based on the Cloudera documentation, but I’ve modified it to be easier to follow for setting up a pseudo cluster and added additional content where necessary. Update: Please be careful if you are copying lines from these articles to paste into your Hadoop config files. I have found that the double hyphen characters used in the comment lines may copy over as a long hyphen instead. This is likely to cause issues when attempting to run the various components.
<!-- more -->
### **Install HBase and Zookeeper**

1.  Open a terminal window on your CentOS server and switch to the root user account with:  **su -**
2.  Enter this command: **yum install hbase** 
3.  Type Y when prompted, and hit enter. After a few seconds the Hbase package will be downloaded and installed.
4.  HBase can use a large number of files at once which may conflict with a system configuration called ulimit that allows a default maximum of 1024 concurrent open files. To increase this limit, as the root user in the terminal window, edit the **/etc/security/limits.conf** file.
5.  At the bottom of the file, before the #End of file line, add these two lines:  
               hdfs             -       nofile  32768  
               hbase           -       nofile  32768
6.  Restart your system to allow the changes to take effect.
7.  Edit the **/etc/hadoop/conf/hdfs-site.xml** file to allow your data node to serve a larger amount of files by adding the following property section to the file. (4096 is the minimum you should use and xcievers is spelled correctly)  
            <property>  
              <name>dfs.datanode.max.xcievers</name>  
              <value>4096</value>  
            </property>
8.   Restart HDFS by either restarting your server, or you can use this command:  
          **for x in \`cd /etc/init.d ; ls hadoop-hdfs-\*\` ; do sudo service $x restart ; done**
9.  Now install the HBase Master which controls the HBase system,and several other servers. Once again at the command line as root, enter the following:  
     **yum install hbase-master**
10.  Once the install completes, you can start the service with this command:  
          **service hbase-master start**
11.  Open a web browser and point it to your your system name and port 60010. For example [http://HadoopTest:60010](http://hadooptest:50070/) . In CDH version 5, you'll see a website with multiple tabs at the top that you can click through to check out the status of your HBase system.
12.  Scroll down to the bottom of the page, and there should be one entry under the Region Servers section.
13.  Now you need to install a Thrift server and a REST server to communicate with HBase. As root in a terminal window, enter this command: **          yum install hbase-thrift**  
    and then start it with: **        service hbase-thrift start**
14.  Once the Thrift install completes, enter this command: **        yum install hbase-rest.**
15.  Before you run the REST server, we need to make some additional configuration changes. At this point you have HBase configured in Standalone mode. It will work fine as it is, but if your are running on one server in a Pseudo-distributed mode, you may want to change HBase to operate in a similar fashion. This will allow each of the component processes to run in a separate Java Virtual Machine.
16.  Edit the config file **/etc/hbase/conf/hbase-site.xml**
17.  The first change is optional. HBase by default uses port 8080, which is a commonly used one and one you may want to change. To use port 60050 add this property section between the configuration tags:  
     **<property>**  
     **<name>hbase.rest.port</name>**  
     **<value>60050</value>**  
     **</property>**
18.  Now add these two additional properties, still inside the configuration tags:  
     **<property>**  
     **<name>hbase.cluster.distributed</name>**  
     **<value>true</value>**  
     **</property>**  
     **<property>**  
     **<name>hbase.rootdir</name>**  
     **<value>hdfs://myhost:8020/hbase</value>**   
     **</property>**
19.  **One gotcha here is the value for the hbase.rootdir hostname needs to match the value in /etc/hadoop/conf/core-site.xml's fs.default.name or fs.defaultFS.**
20.  Save the file. Now you need to create the hbase directory in HDFS and give ownership to the HBase account, by entering these two lines (be sure to use the sudo - u hdfs portion to make sure the command is run by the correct Hadoop user):  
             **sudo -u hdfs hadoop fs -mkdir /hbase**  
             **sudo -u hdfs hadoop fs -chown hbase /hbase**
21.  You are almost down with HBase, but before we finish, you need to install Zookeeper and configure it to run on a single server. Install the package by entering at the command line as root: **yum install zookeeper-server**
22.  Create the Zookeeper folders in the Linux file system and change permissions by entering the following:  
             **mkdir -p /var/lib/zookeeper**  
            ** chown -R zookeeper /var/lib/zookeeper/**
23.  Now you can initialize and start Zookeeper, by entering these commands as root:  
     **service zookeeper-server init** (You'll receive a message about specifying a myid if you are running in non-standalone mode. Its safe to ignore it).  
     **service zookeeper-server start**
24.  Because this is a pseudo-distributed mode installation primarily for testing and development, you are OK with using a single Zookeeper server. If this were a production setup, you would want to install an odd number of Zookeeper servers to ensure reliability.
25.  Finally we can start the HBase Master! Enter the following command to start it up:  
     **service hbase-master start**
26.  At this point we are almost finished. We just need to install the RegionServer (that's the part of HBase that hosts the data and actually handles requests). To install your region server, type the following command as root:  
     **yum install hbase-regionserver**
27.  Once it completes installing, start it with this command:  
     **service hbase-regionserver start**
28.  And then start the HBase Rest service with this command:   
    
     **service hbase-rest start**
    
29.  Reopen a web browser and point it to your your system name and port 60010. Click through the various tabs to check out the status of your HBase system.

### **SNAPPY configuration**

Snappy is installed as part of the initial Hadoop installation. Its a compression library used by MapReduce, Hive, HBase, Sqoop and Pig and can make applications run faster without any changes to the application code.

1.  To enable Snappy for MapReduce, from a command line as root, edit: **/etc/hadoop/conf/mapred-site.xml** and add these sections to the file before the final </configuration> tag:  
            **<!-- Enable Snappy for MapReduce -->**  
     **<property>**  
     **<name>mapred.compress.map.output</name>**   
     **<value>true</value>**  
     **</property>**  
     **<property>**  
     **<name>mapred.map.output.compression.codec</name>**   
     **<value>org.apache.hadoop.io.compress.SnappyCodec</value>**  
     **</property>**
2.  Configurations for other components are enabled in the code to call the specific job.
3.  Restart your server for Snappy to take effect.