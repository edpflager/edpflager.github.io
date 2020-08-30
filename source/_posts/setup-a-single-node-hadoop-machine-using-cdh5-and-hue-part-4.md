---
title: Setup a Single-node Hadoop machine using CDH5 and HUE – Part 4
tags:
  - CDH
  - Hadoop
  - howto
id: '1985'
categories:
  - - Big Data
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2014-04-21 15:43:43
---

[![oozie-derby](http://edpflager.com/wp-content/uploads/2014/04/oozie-derby.png)](http://edpflager.com/wp-content/uploads/2014/04/oozie-derby.png)This is part 4 of my series on how to setup a Hadoop single node, pseudo-cluster using Cloudera’s CDH distribution. In [part 1](http://edpflager.com/?p=1945 "Setup a Single-node Hadoop machine using CDH5 and HUE – Part 1"), I walked through the beginning steps to configure a CentOS 6.5 system for use as a single-node Hadoop pseudo-cluster. In [part 2](http://edpflager.com/?p=1964 "Setup a Single-node Hadoop machine using CDH5 and HUE – Part 2"), I walked through installing and configuring HBase, Zookeeper and Snappy. [Part 3](http://edpflager.com/?p=1973 "Setup a Single-node Hadoop machine using CDH5 and HUE – Part 3") covered installing HUE - the graphical front end from Cloudera that makes interacting with the various components in our cluster easier.  In this installment, we'll set up Oozie - a job scheduler component for Hadoop, and fix a minor problem with the Hue setup. This series is based on the Cloudera documentation, but I’ve modified it to be easier to follow for setting up a pseudo cluster. Update: Please be careful if you are copying lines from these articles to paste into your Hadoop config files. I have found that the double hyphen characters used in the comment lines may copy over as a long hyphen instead. This is likely to cause issues when attempting to run the various components.
<!-- more -->
### Install Oozie

1.  Open a terminal window and switch to the root account.
2.  Start the installation of the Oozie server and client tools with this command: **       yum install oozie**
3.  By default, Oozie uses a Derby database. (If you aren't familiar with Derby, its a relational database management system from Apache, implemented in Java. It has very minimal operational requirements, making it ideal for implementing as part of other applications). Derby has some limitations, although as part of a  pseudo cluster it should be adequate.
4.  We are using MRv1 with this setup, and no SSL because its a development/test system. To let Oozie know, enter the following command: **alternatives --set oozie-tomcat-conf /etc/oozie/tomcat-conf.http.mr1**
5.  Oozie has a web console for management, but because we are using HUE for our setup, its not necessary, so we won't configure it.
6.  Next, create the Oozie database in Derby by entering this command:      **sudo -u oozie /usr/lib/oozie/bin/ooziedb.sh create -run**
7.  Create the Oozie user folder in your Hadoop file system , and then change the owner to Oozie: **     sudo -u hdfs hadoop fs -mkdir /user/oozie sudo -u hdfs hadoop fs -chown oozie:oozie /user/oozie**
8.  Finally to install the shared library, enter this command: **oozie-setup sharelib create -fs hdfs://localhost -locallib /usr/lib/oozie/oozie-sharelib-mr1.tar.gz**
9.  By default Oozie does not allow MapReduce jobs with uber jars. (An uber-jar is a "super jar", one that packages both your code package and all its dependencies into one single JAR file.) If you think you need the ability to use uber jar with your pseudo machine, then Edit the file: **/etc/oozie/conf/oozie-site.xml** and add this property at the top just after the <configuration> tab: **<property> <name>oozie.action.mapreduce.uber.jar.enable</name> <value>true</value>** **     </property>**
10.  Start Oozie with the command: **service oozie start**
11.  Open the HUE interface (**http://localhost:8888**), refresh it, and view the configuration issues. If everything was setup Ok with Ooze, you’ll see that the errors related to Oozie are now gone.
12.  Open the Oozie web interface (**http://localhost:11000**) and you should see a message that the Oozie Web Console is disabled.

**HUE Secret Key** Earlier in this series, we saw in the HUE.ini file a placeholder for entering a secret key. This is used as a salt value to generate a security HASH. You can leave this empty, but if you do, the HUE console will let you know about it every time you enter the HUE web interface. Its very easy to fix, so we'll walk through it now.

1.  Open a terminal window, and switch to your root account. (Of course!)
2.  Edit the Hue configuration file: **/etc/hue/conf.empty/hue.ini**
3.  The first section of the file is the General configuration. Find the \[desktop\] subsection, and look for the secret\_key parameter. Currently it should be: **secret\_key=**
4.  Enter a random string of characters after the equals sign (20-30 should be sufficient). Save the file and exit back to the command prompt.
5.  Restart Hue with the service restart command: **service hue restart**
6.  Switch back over to the Hue web interface and re-login. The error message about secret\_key should now be gone.

That's all for this installment. Next time, we'll look at getting Hive2 up and running and integrated into HUE. After that is the Impala installation, Spark, and SQOOP.