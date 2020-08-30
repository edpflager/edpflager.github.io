---
title: Setup a Single-node Hadoop machine using CDH5 and HUE – Part 6
tags:
  - CDH
  - Cloudera
  - Hadoop
  - howto
  - impala
id: '2024'
categories:
  - - Big Data
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2014-04-28 15:35:14
---

[![Männliche Impala in der Serengeti](http://edpflager.com/wp-content/uploads/2014/04/Impala-Sqoop.jpg)](http://edpflager.com/wp-content/uploads/2014/04/Impala-Sqoop.jpg)This is part 6 in my series on setting up a Hadoop single node, pseudo-cluster using Cloudera’s CDH distribution. In [part 1](http://edpflager.com/?p=1945 "Setup a Single-node Hadoop machine using CDH5 and HUE – Part 1"), I walked through the beginning steps to configure a CentOS 6.5 system for use as a single-node Hadoop pseudo-cluster. In [part 2](http://edpflager.com/?p=1964 "Setup a Single-node Hadoop machine using CDH5 and HUE – Part 2"), I walked through installing and configuring HBase, Zookeeper and Snappy. [Part 3](http://edpflager.com/?p=1973 "Setup a Single-node Hadoop machine using CDH5 and HUE – Part 3") covered installing HUE – the graphical front end from Cloudera that makes interacting with the various components in our cluster easier.  [Part 4](http://edpflager.com/?p=1985 "Setup a Single-node Hadoop machine using CDH5 and HUE – Part 4") as about the set up of  Oozie – a job scheduler component for Hadoop, and how to fix a minor problem with the Hue setup involving a secret key. Last time, in [Part 5](http://edpflager.com/?p=2003 "Setup a Single-node Hadoop machine using CDH5 and HUE – Part 5"),  installing and configuring Hive2 and getting it to work with Cloudera’s HUE web interface was covered.

In this article, I'll show the steps necessary to integrate two tools for working with structured SQL data in a Hadoop pseudo-cluster environment: Impala - Cloudera's query tool for Hadoop, and Sqoop2 - an ETL tool to move data between relational database systems and Hadoop. This series is based on the Cloudera documentation, but I’ve modified it to be easier to follow for setting up a pseudo cluster.

Update: Please be careful if you are copying lines from these articles to paste into your Hadoop config files. I have found that the double hyphen characters used in the comment lines may copy over as a long hyphen instead. This is likely to cause issues when attempting to run the various components.
<!-- more -->
### INSTALLING IMPALA

According to Cloudera's [website](http://www.cloudera.com/content/cloudera/en/products-and-services/cdh/impala.html), "Impala is a massively parallel processing query engine that runs natively in Apache Hadoop." As more and more organizations are looking to use Hadoop to handle the vast amounts of data, tools that can be used that work with a SQL like syntax have become more readily available. Impala is one, HIVE is another, but they aren't the only ones: Hortonworks has open-sourced Stinger, Facebook  provides Presto, and  HAWQ from Pivotal are just a few.

1.  Before installing Impala, you need to add the Impala Repository information to your system. As root at a terminal prompt, change to the **/etc/yum.repos.d** folder.
2.  Download the Impala Repo information from Cloudera, with this command: **      wget http://archive.cloudera.com/impala/redhat/6/x86\_64/impala/cloudera-impala.repo**
3.  Now to install the various Impala components, run these commands: **     yum install impala** **     yum install impala-server** **     yum install impala-state-store** **     yum install impala-catalog**
4.  Once the components are installed, edit **/etc/hadoop/conf/hdfs-site.xml** and add these properties to it: **     <property>          ** **<name>dfs.client.read.shortcircuit</name>** **<value>true</value>** **     </property>** **<property>** **<name>dfs.domain.socket.path</name>** **          <value>/var/run/hadoop-hdfs/dn.\_PORT</value>  ** **     </property>** **<property>** **<name>dfs.client.file-block-storage-locations.timeout</name>** **<value>30000</value>** **          <--- Cloudera's website says 3000 but that caused my impala-server to shutdown because the value was too low. -->            ** **     </property>** **<property>** **          <name>dfs.datanode.hdfs-blocks-metadata.enabled</name>          ** **<value>true</value>** **     </property>**
5.  Save the file and exit. Now copy several config files to the **/etc/impala/conf** folder. (I have not tried using symbolic links for this, but it may be possible.) **cp /etc/hive/conf/hive-site.xml /etc/impala/conf** **cp /etc/hadoop/conf/core-site.xml /etc/impala/conf** **     cp /etc/hadoop/conf/hdfs-site.xml /etc/impala/conf**
6.  Change the group owner on **/var/run/hadoop-hdfs** to root: **     chgrp root /var/run/hadoop-hdfs**
7.  Finally, install the Impala Shell application with this command: **yum install impala-shell**
8.  Go ahead and restart the server (for me that is easier than restarting all of the various services). At this point you will not have any database connections defined for Impala. Please see Cloudera's website for information on setting up [ODBC](http://www.cloudera.com/content/cloudera-content/cloudera-docs/CDH5/latest/Impala/Installing-and-Using-Impala/ciiu_impala_odbc.html) connections and [JDBC](http://www.cloudera.com/content/cloudera-content/cloudera-docs/CDH5/latest/Impala/Installing-and-Using-Impala/ciiu_impala_jdbc.html) connections.
9.  When your server comes back up, login to HUE and the error messages about Impala should be gone and you should be able to access Impala from the menu.

INSTALLING Sqoop2 Apache Sqoop2 is useful when you have data that you need to interact with in a Hadoop environment that comes from or will be pushed to a relational database management (RDBMS) systems. In addition, because Sqoop2 moves the data in a bulk load fashion, it can save time when bringing a very large amount of data to a Hadoop cluster for processing, even with the addition of the movement of data.

1.  Open a terminal window and change to root user.
2.  Enter this command to install the sqoop2 server and client packages: **yum install sqoop2-server** 
3.  Sqoop2 can be used with MapReduce v1 or v2 (YARN), but not at the same time. Configuration files for use with the Tomcat webserver for both are provided as part of the installation, so you need to define which one to use. Add an alternatives entry for it (that's two dashes before the word SET): **   alternatives --set sqoop2-tomcat-conf /etc/sqoop2/tomcat-conf.mr1**
4.  Start the Sqoop2 service: **     service sqoop2-server start**
5.  Restart HUE: **service hue restart**

Sqoop2 should now be accessible from the menu in Hive as well. Next time, we'll finish up setting up the single node Hadoop server with CDH5 and HUE by looking at Apache Spark. Its a general purpose engine for running applications in a Hadoop environment and achieves speeds considerably faster than using native MapReduce.