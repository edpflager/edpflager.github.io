---
title: Connecting Kettle to Cloudera Hadoop Impala
tags:
  - CDH
  - Cloudera
  - ETL
  - Hadoop
  - How-to
  - howto
  - impala
  - kettle
  - PDI
  - SysAdmin
  - technical
id: '2592'
categories:
  - - Big Data
  - - Blog
  - - Business Intelligence
  - - Linux
  - - Pentaho
comments: false
date: 2014-12-05 18:44:35
---

[![hadoop-elephant](http://edpflager.com/wp-content/uploads/2014/12/hadoop-elephant-300x214.png)](http://edpflager.com/wp-content/uploads/2014/12/hadoop-elephant.png)As Big Data platforms like Hadoop and its ecosystem of related applications has matured, they have moved beyond the original key-value model to embrace data processing of more traditional structured data. But a big problem for DBAs and Data Analysts wanting to use the power of these new platforms to analyze data from RDBMS systems like MySQL, SQL Server, is getting data moved between them. Using CSV or flat files is one way, but it adds additional processing. Data has to be extracted from the source system to an intermediary format and then imported into the destination. Its far more efficient and less prone to error if the data can be passed without that middle step. In this first article of a series where I'll be looking at interactivity between Hadoop and other database systems, I'll cover setting up a database connection to Hadoop via Cloudera's Impala JDBC driver to Pentaho's Kettle ETL system.
<!-- more -->
##### Prerequisites:

*   If you want to follow along, and you don't have a Hadoop environment setup to play with, download a [Cloudera Quick Start VM](http://www.cloudera.com/content/cloudera/en/downloads/quickstart_vms/cdh-5-2-x.html) to work with. There are versions available for VMWare/Fusion, Oracle's VirtualBox and KVM.
*   Additionally, you'll need a recent copy of the community version of [Pentaho Data Integration](http://sourceforge.net/projects/pentaho/files/Data%20Integration/5.2/pdi-ce-5.2.0.0-209.zip/download) (aka Kettle). I don't recommend installing Kettle in the VM that you are running Hadoop in, but if your host machine is sufficiently powerful, you can run it there alongside the VM.
*   Once you have those applications in place, download the Cloudera JDBC driver for Impala [here](https://downloads.cloudera.com/impala-jdbc/impala-jdbc-0.5-2.zip). The current version as of this writing is 0.5.2. The driver is O/S independent, so it will work with Linux/MacOSX/Windows.
    

1.  Extract the JDBC ZIP file to somewhere you can access, and then copy the nine JAR files to the data-integration/lib folder for Pentaho. Because there is already a log4j jar file present in the LIB folder, you may get a prompt to overwrite it. Go ahead.
2.  Start up the Hadoop VM. Open a terminal prompt and enter the command: **ifconfig** to get the IP address assigned to the VM.![IPAddress](http://edpflager.com/wp-content/uploads/2014/12/IPAddress-300x112.png)
3.  Switch to the PC where you have Pentaho installed. Open a terminal window on that machine and enter this command: ping <ip address>, substituting the address you got in step 2 above. If the results are not successful you'll need to determine what is causing the breakdown in communications. Unfortunately that is outside the scope of this article but two possibilities may be the firewall/ip tables is turned on in your VM, or the network connection in your VM platform may need to be configured differently.![Ping](http://edpflager.com/wp-content/uploads/2014/12/Ping-300x74.png)
4.  Start Pentaho Data Integration and start a new transformation. Switch to the View tab in the left panel of the application. Expand the Transformation folder by toggling the folder arrow, and then the Transformation. Right click the Database connections option and click NEW in the menu that appears.
5.  The Database Connection window will appear.
6.  Enter a name in the Connection Name box to identify it.
7.  Scroll down in Connection Type and choose Impala.
8.  In the Access panel, the Native (JDBC) should be selected since its the only option.
9.  In the Settings panel, enter your VM's IP address, the database you want to connect to (for our purposes we'll use the default database, called default) and the user name: cloudera and password: cloudera in the appropriate fields. The port will default to 21500. Unless you have modified the Impala port, you can leave it set to 21500.
10.  Once you are finished, the window should like the image below. Click the test button, and you should receive a message that the connection was successful.[![DBConnection](http://edpflager.com/wp-content/uploads/2014/12/DBConnection-300x279.png)](http://edpflager.com/wp-content/uploads/2014/12/DBConnection.png)[](http://edpflager.com/wp-content/uploads/2014/12/connection.png)
11.  Congratulations! You have made a database connection to your test Hadoop environment!