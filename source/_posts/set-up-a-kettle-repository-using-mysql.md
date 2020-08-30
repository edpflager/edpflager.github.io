---
title: 'Updated: Set up a Kettle repository using MySQL'
tags:
  - ETL
  - kettle
  - PDI
id: '1622'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
  - - Pentaho
comments: false
date: 2013-08-17 12:32:12
---

[![kettle](http://edpflager.com/wp-content/uploads/2013/08/kettle-260x300.jpg)](http://edpflager.com/wp-content/uploads/2013/08/kettle.jpg)When creating ETL workflows, its useful to store the information in a database repository, rather than as individual files on your workstation. This allows multiple users to have access to the information (why recreate the wheel?),  it allows you to pull it into your jobs quickly and easily, and you can back it up quickly and restore it if necessary. Pentaho Data Integration (aka Kettle) is an open source ETL tool that has a repository feature, which allows you to store your transformations and jobs in local files, or in a central repository database. The file option is pretty easy to implement, so I won't cover it here. Because of my work experience, I prefer to use a database server based repository. Unfortunately, the documentation for setting up a DB repository is sorely lacking (a common problem with a lot of open source projects). After some experimenting, I did figure out how to create a MySQL based repository, and how to connect to it from a Linux based installation of PDI. Here is a walk through of the process:
<!-- more -->
Prerequisites:

*   Make sure you have MySQL installed and you can access it from your workstation.
*   Create (or have your sysadmin create) an empty database, and create a user account with permissions on the database to add/delete/etc.
*   On the Linux machine where you have Pentaho installed, download the MySQL Connector/J driver. ([http://dev.mysql.com/downloads/connector/j/](http://dev.mysql.com/downloads/connector/j/)). Extract it and locate the mysql-connector-java-XXXX-bin.jar file. Copy that file to the pentahodata-integrationlibextJDBC subfolder.
*   **Update: In Pentaho 5, the location for the JAR file has moved. Its should now be copied into pentahodata-integrationlib.**

Create a Kettle(PDI) repository Start PDI on your system. When it first loads up, you will see a Repository Connection screen. Locate the small green circle with the plus sign, and click on it to add a new repository. [![add_repository](http://edpflager.com/wp-content/uploads/2013/08/add_repository-300x188.png)](http://edpflager.com/wp-content/uploads/2013/08/add_repository.png) In the Select the repository type window, select the first option: Kettle database repository, and then click OK. ![repository_type2](http://edpflager.com/wp-content/uploads/2013/08/repository_type2-300x93.png) For the Repository information window, click the NEW button, to create a new connection. ![repository_dbconnection](http://edpflager.com/wp-content/uploads/2013/08/repository_dbconnection-300x87.png) You'll see the Database Connection window now. This is a pretty busy window, but we are almost done: On the General tab, enter a Connection Name. I use "pentaho". Scroll through the Connection Type panel and click on MySQL (if you are using MariaDB, use MySQL as well). In the Access panel, choose Native(JDBC). In the settings panel, enter the host name or IP address of your database server. If you are running it on the same machine you have PDI installed on, just enter localhost. Enter the database name that was created as part of the requirements section ( I used pentaho). Leave the port set to 3306, unless your sysadmin has changed the default MySQL port. Enter the User Name and Password set up as part of the requirements section. Click OK. ![repository_dbconnection2](http://edpflager.com/wp-content/uploads/2013/08/repository_dbconnection2-300x253.png) You'll be back to the Repository Information window now. Enter an ID and a Name for your connection in the appropriate box. Finally, click on the Create or Upgrade button. At the ARE YOU SURE? prompt, click on the YES button. At the Dry Run prompt, you can click the NO prompt unless you are curious about the SQL that PDI will generate to create the repository tables. ![DryRun](http://edpflager.com/wp-content/uploads/2013/08/DryRun-300x143.png) A progress window will appear for a few seconds, and when it finishes a message will appear on the screen letting you know. ![created repository](http://edpflager.com/wp-content/uploads/2013/08/created-repository-300x105.png) Click OK to return to the Repository Information window again. Click OK to go back to the Repository Connection window. You new connection should be here. Click the name of your repository, and enter the password for the admin account. (It's 'admin'). PDI will finish loading, and you'll be connected to your repository where your transformations and jobs will be saved.