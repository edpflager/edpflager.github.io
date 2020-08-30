---
title: Setup a Single-node Hadoop machine using CDH5 and HUE – Part 5
tags:
  - CDH
  - Hadoop
  - howto
  - install
  - MySQL
id: '2003'
categories:
  - - Big Data
  - - Blog
  - - Linux
comments: false
date: 2014-04-25 15:28:11
---

[![hivemysql](http://edpflager.com/wp-content/uploads/2014/04/hivemysql.png)](http://edpflager.com/wp-content/uploads/2014/04/hivemysql.png)This is part 5 in my series on setting up our a Hadoop single node, pseudo-cluster using Cloudera’s CDH distribution. In [part 1](http://edpflager.com/?p=1945 "Setup a Single-node Hadoop machine using CDH5 and HUE – Part 1"), I walked through the beginning steps to configure a CentOS 6.5 system for use as a single-node Hadoop pseudo-cluster. In [part 2](http://edpflager.com/?p=1964 "Setup a Single-node Hadoop machine using CDH5 and HUE – Part 2"), I walked through installing and configuring HBase, Zookeeper and Snappy. [Part 3](http://edpflager.com/?p=1973 "Setup a Single-node Hadoop machine using CDH5 and HUE – Part 3") covered installing HUE – the graphical front end from Cloudera that makes interacting with the various components in our cluster easier. Last time in [Part 4](http://edpflager.com/?p=1985 "Setup a Single-node Hadoop machine using CDH5 and HUE – Part 4"), I walked through the set up of  Oozie – a job scheduler component for Hadoop, and how to fix a minor problem with the Hue setup involving a secret key.

In this article, I'll walk through installing and configuring Hive2 and getting it to work with Cloudera's HUE web interface. This series is based on the Cloudera documentation, but I’ve modified it to be easier to follow for setting up a pseudo cluster.

Update: Please be careful if you are copying lines from these articles to paste into your Hadoop config files. I have found that the double hyphen characters used in the comment lines may copy over as a long hyphen instead. This is likely to cause issues when attempting to run the various components.
<!-- more -->
### **CREATE A HIVE  FOLDER IN HDFS**

If you have followed along with this series, when you open the HUE web interface that is connected to your pseudo-cluster, you should see this configuration error:

**Hive Editor - Failed to access Hive warehouse: /user/hive/warehouse**

Its means that HUE can't get to the Hive warehouse folder in HDFS, and its a very easy problem to fix. As we've done previously, open up a terminal window, and switch over to the root account with: **su -**

1.  Create the hive user folder in hdfs, and a warehouse subfolder, then change the ownership by running these three commands, one after another:  **sudo -u hdfs hadoop fs -mkdir /user/hive** **sudo -u hdfs hadoop fs -mkdir /user/hive/warehouse** **           sudo -u hdfs hadoop fs -chmod 1777 /user/hive/warehouse**
2.  Restart the HUE service and re-login:  **service hue restart**

### **INSTALL HIVE SERVER 2**

The previous error will be gone, and it will be replaced with a new one: **Hive Editor - The application won't work without a running Hive Server2.**

Unfortunately, the fix for this problem is a little more involved. Earlier in this series, one of the components we were dealing with was using a Derby database. While Hive Server 2 will also work with Derby, I've had better results using MySQL as a backend for Hive, so we'll install that and configure it for Hive.

1.  In a terminal window as root, install MySQL:  **yum install mysql-server**
2.  Start the service with this command:  **service mysqld start**
3.  Now install the JDBC connector package for MySQL and copy the JAR file to the hive home folder:  **yum install mysql-connector-java**  **cp  /usr/share/java/mysql-connector-java.jar /usr/lib/hive/lib/mysql-connector-java.jar**
4.  Configure MySQL to make it more secure by running the following command: **/usr/bin/mysql\_secure\_installation** At the prompts you should: set a new password, remove anonymous users, allow remote root login, remove test database, and reload privilege tables.
5.  Configure your system to start MySQL at startup:  **/sbin/chkconfig mysqld on**
6.  And verify that the settings are OK. The results of the following command should indicate that run-levels 2 through 5 are set to ON.  **/sbin/chkconfig --list mysqld**
7.  Hive 2 requires a database to store some of its information in, so create it:  **mysql -u root -p** You'll be prompted to enter the password  you set in step 4 above. At the mysql> prompts, enter the following commands to create a database and switch to it: **CREATE DATABASE metastore;**  **USE metastore;**
8.  Tell MySQL to create the structure of the database using this command at the mysql> prompt: **SOURCE /usr/lib/hive/scripts/metastore/upgrade/mysql/hive-schema-0.12.0.mysql.sql;**
9.  Finally, add a local hive user to the database and assign permissions to it. This account will only be able to access the database from the local machine:  **CREATE USER 'hive'@'localhost' IDENTIFIED BY 'mypassword’;**  **REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'hive'@'localhost';**  **GRANT SELECT,INSERT,UPDATE,DELETE,LOCK TABLES,EXECUTE ON metastore.\* TO 'hive’@‘localhost’; FLUSH PRIVILEGES;** **QUIT;**
10.  MySQL is now installed with a database and user for Hive to use. Now let's setup Hive2.

### HIVE 2 SETUP

1.  CDH5 has replaced the original version of HIVE with HIVE2. In order to make it work with CDH5 and HUE, run the following commands to install the  components (as root in a terminal windows): **yum install hive-metastore** **service hive-metastore start** **yum install hive-server2** **service hive-server2 start** **     yum install hive-hbase**
2.  Edit **/etc/hive/conf/hive-site.xml** to change connection info to mySQL. The first two properties should already be in the file, and will need to be modified: <property>     <name>javax.jdo.option.ConnectionURL</name>     <value>jdbc:mysql://localhost/metastore_?createDatabaseIfNotExist=true<_/value> <description>the URL of the MySQL database</description> </property><property> <name>javax.jdo.option.ConnectionDriverName</name> <value>com.mysql.jdbc.Driver</value> </property><property> <name>javax.jdo.option.ConnectionUserName</name> <value>hive</value> </property> <property> <name>javax.jdo.option.ConnectionPassword</name> <value>password you created in step 9 for MySQL</value> </property>
3.  Save the file, and restart your server.
4.  After your server reboots, open a terminal window and switch to the root account. Check the hive metastore service:  **service hive-metastore status** If it responds dead, then you need to make sure the java-connector has been copied correctly to **/usr/lib/hive/lib/mysql-connector-java.jar**
5.  Login to Hue, and the error about Hive Server 2 should be gone.

Almost complete! Next time I'll cover setting up Impala with our pseudo cluster, and talk about Spark.