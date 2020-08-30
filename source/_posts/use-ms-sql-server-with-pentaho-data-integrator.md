---
title: Use MS SQL Server with Pentaho Data Integrator
tags:
  - ETL
  - howto
  - install
  - kettle
  - PDI
  - technical
id: '2130'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
  - - Pentaho
comments: false
date: 2014-06-13 13:33:38
---

[![etl](http://edpflager.com/wp-content/uploads/2013/08/etl-300x111.png)](http://edpflager.com/wp-content/uploads/2013/08/etl.png)Being an open-source tool, I'm sure a large number of Pentaho Data Integrator (Kettle) users are working with open-source database systems like MySQL, MariaDB, Hadoop, MongoDB or PostgreSQL. But it also works well with with some of the Big Boys of the closed-source database world too. In my case, I often use Kettle to connect to Microsoft SQL Server and I know there are some gotchas you need to overcome to get it to work, especially if you are using Kettle on Mac OSX or Linux.
<!-- more -->
###### Gotchas

1.  For better performance whenever possible use a JDBC (java database connector) driver instead of an ODBC (open database connector) driver. Because Kettle is a Java application, it works better with JDBC. So where do you get a JDBC driver for SQL Server? Currently the official Microsoft version is available [here](http://msdn.microsoft.com/en-us/data/aa937724.aspx). From the Microsoft wesbite, click the Download link, and get the sqljdbc\_4.0.2206.100\_enu.tar.gz file.Extract the contents, and look for the sqljdbc4.jar file in the root of the extracted folder.  On Mac OSX, copy that file to the data-integrationlib folder. On my Mac that path is under ApplicationsPentahodata-integrationlib. On Linux, it will depend on where you installed Kettle. In my case its currently under my home folder, and then data-integrationlib as well. If you are using the PDI server, you will also need to copy the jar file to the   /pentaho/server/data-integration-server/tomcat/webapps/pentaho-di/WEB-INF/lib/ folder.
2.  If the Microsoft Windows server that SQL Server is running on has the Windows Firewall turned on, an incoming rule needs to be in place to allow connections to the SQL Server. Generally if the server is in production use, this will be  already done, but if not, it will need to be setup (for instance a new SQL installation or a development server). Microsoft has documentation for this on their [website](http://msdn.microsoft.com/en-us/library/ms175043(v=sql.110).aspx) if you are the server administrator, or contact your server administrator to set it up for you.
3.  The account you use to connect to the database is dependent on how SQL Server authentication was configured. SQL Server can use:

*   Active Directory (AD) authentication - user IDs and passwords are checked against an external repository of users tied to your network, or
*   Mixed Mode - locally defined user accounts on the server and AD authentication. I will show you how to setup a connection to a SQL server setup in Mixed Mode in this article. Configuring for AD authentication is a more involved process, and will be covered in a future article.To follow this tutorial, have the SQL Administrator create a local user account on the SQL Server instance with read permissions to the database(s) that you need to access with Kettle.

###### Configuration

1.  To create your database connection in the Spoon GUI, start a new transformation (File ->New ->Transformation).
2.  Switch to the View tab in the left panel and right click on Database Connections. Click New in the menu that appears.
3.  The Database Connection window will appear. Enter a Connection Name in the top field.
4.  Select MS SQL Server in the Connection Type area.
5.  In the Access area select MS SQL Server. There are two options here.
6.  Now you need to populate the various Settings fields.If your DNS is working correctly or you have entered your SQL Server's IP address and name in your Hosts file, you can enter the hostname in the Host Name field. Otherwise just enter the IP address.
7.  Enter the database name you will be using in that field.
8.  The Instance Name box can be left empty unless your SQL Server is setup with a distinct instance (your SQL DBA can tell you).
9.  The port will default to MSSQL's normal one: 1433.
10.  Finally enter your SQL Server user account and password.
11.  Your connection screen should look similar to what I have pictured here.[![DBConnection](http://edpflager.com/wp-content/uploads/2014/06/DBConnection-300x279.png)](http://edpflager.com/wp-content/uploads/2014/06/DBConnection.png)
12.  Click the TEST button and if everything is configured correctly, you should see a small window popup telling you that connection was successful!