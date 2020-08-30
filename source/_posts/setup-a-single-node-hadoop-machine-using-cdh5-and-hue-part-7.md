---
title: Setup a Single-node Hadoop machine using CDH5 and HUE – Part 7
tags:
  - CDH
  - Cloudera
  - Hadoop
  - howto
  - technical
id: '2058'
categories:
  - - Big Data
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2014-05-02 15:51:55
---

[![nospark](http://edpflager.com/wp-content/uploads/2014/05/nospark.png)](http://edpflager.com/wp-content/uploads/2014/05/nospark.png)This is part 7 (the last part) in my series on setting up a Hadoop single node, pseudo-cluster using Cloudera’s CDH distribution. In [part 1](http://edpflager.com/?p=1945 "Setup a Single-node Hadoop machine using CDH5 and HUE – Part 1"), I walked through the beginning steps to configure a CentOS 6.5 system for use as a single-node Hadoop pseudo-cluster. In [part 2](http://edpflager.com/?p=1964 "Setup a Single-node Hadoop machine using CDH5 and HUE – Part 2"), I walked through installing and configuring HBase, Zookeeper and Snappy. [Part 3](http://edpflager.com/?p=1973 "Setup a Single-node Hadoop machine using CDH5 and HUE – Part 3") covered installing HUE – the graphical front end from Cloudera that makes interacting with the various components in our cluster easier.  [Part 4](http://edpflager.com/?p=1985 "Setup a Single-node Hadoop machine using CDH5 and HUE – Part 4") as about the set up of  Oozie – a job scheduler component for Hadoop, and how to fix a minor problem with the Hue setup involving a secret key. Installing and configuring Hive2 and getting it to work with Cloudera’s HUE web interface was covered in [Part 5](http://edpflager.com/?p=2003 "Setup a Single-node Hadoop machine using CDH5 and HUE – Part 5"). And last time in Part 6, I went through the steps necessary to integrate two tools for working with structured SQL data in a Hadoop pseudo-cluster environment: Impala - Cloudera's query tool for Hadoop, and Sqoop2 - an ETL tool to move data between relational database systems and Hadoop. With this article, we'll wrap up the series by looking at Apache Spark. Its a general purpose engine for running applications in a Hadoop environment and achieves speeds considerably faster than using native MapReduce. And my little disclaimer: this series is based on the Cloudera documentation, but I’ve modified it to be easier to follow for setting up a pseudo cluster. Update: Please be careful if you are copying lines from these articles to paste into your Hadoop config files. I have found that the double hyphen characters used in the comment lines may copy over as a long hyphen instead. This is likely to cause issues when attempting to run the various components.
<!-- more -->
If you've followed along with this series, at this point you should have a functioning Pseudo-cluster running on CentOS 6.5 with Cloudera's CDH5 Hadoop distribution. The only problem we still have to tackle is when you login to HUE, you see and a configuration error: "Spark Editor - The app won't work without a running Job Server."

I'm going to show you two ways to deal with this message. The first for me is the easier, and the one I opted for because I am not currently using Spark. I have disabled this functionality on my server.

DISABLE SPARK

1.  Open a terminal window and switch to the root account.
2.  Navigate to **/etc/hue/conf.empty** and open the file **hue.ini** in your editor.
3.  Search for the line that contains: **app\_blacklist=**
4.  Uncomment the line, and add **spark** at the end. It should now read: **       app\_blacklist=spark**
5.  Save the file and exit your editor.
6.  Restart HUE with this command: **     service hue restart**
7.  Login to the HUE inteface, and the warning message will be gone, and the Spark option will no longer appear in the menu either.

HOW TO INSTALL SPARK

If you intend to work with Spark in your cluster, installation is pretty straightforward.

1.  Open a terminal window and switch to the root user.
2.  Install the various Spark packages by entering this command: **yum install spark-core spark-master spark-worker spark-python**
3.  That will take a few minutes to pull all of the packages down from the Internet repository and install them. Once it completes you can start the services with these commands: **      service spark-master start** **      service spark-worker start**
4.  Open a web browser and access port 18080 on your server (localhost) to see if the Spark Master console is displayed: **http://localhost:18080**
5.  At this point you will still need to get Spark integrated with the Hue interface. Again in the terminal window as root, you'll need to download SBT (a Scala environment) with this command: **     wget [http://repo.scala-sbt.org/scalasbt/sbt-native-packages/org/scala-sbt/sbt/0.13.1/sbt.rpm](http://repo.scala-sbt.org/scalasbt/sbt-native-packages/org/scala-sbt/sbt/0.13.1/sbt.rpm)**
6.  And install it: **     rpm -ivh sbt.rpm**
7.  Next install GIT (a version control application): **     yum install git**
8.  Copy the Spark repository to your system: **      git clone https://github.com/ooyala/spark-jobserver.git**
9.  Once that completes, switch to the Spark Jobserver folder: **     cd spark-jobserver**
10.  Start the SBT application: **     sbt (loads the SBT application)**
11.  And start the Spark Job Server within SBT, which will take awhile: **     re-start**
12.  This command will not exit back to a prompt, but stays open while Spark runs. Open another terminal window, and switch to root. Restart the HUE service: **     service hue restart**
13.  And log back into HUE. The error message about Spark will be gone. To restart Spark if your system is rebooted, you will need to rerun steps 9-12 above to get it running again. I'm sure there is a way to automate it, but since I'm not using Spark, I haven't investigated it. I leave that as an exercise for the reader.

So that's the end for this series! If you've made it all the made through and everything is working well, congratulations to you! The process of deciphering various webpages instructions took me a couple of weeks to get through so this is definitely not a process for the timid. Hopefully this has made it a bit easier for you.