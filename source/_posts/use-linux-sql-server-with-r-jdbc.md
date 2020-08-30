---
title: Use Linux SQL Server with R (JDBC)
tags:
  - cookbook
  - guides
  - How-to
  - howto
  - SQL Server
  - SysAdmin
  - technical
  - Ubuntu
id: '3537'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2017-04-29 04:30:00
---

[![](http://edpflager.com/wp-content/uploads/2017/04/Rlogo-150x150.png)](http://edpflager.com/?attachment_id=3545#main)Over the past few months, I have been working to learn [R, a free software environment](https://www.r-project.org/) for statistical computing. Its been gaining popularity over the past few years, and Microsoft just gave it a huge boost by integrating R into their Power BI visualization software and the Windows version of SQL Server 2016. Since a good deal of my work involves connecting to Microsoft SQL Servers and a Linux version of SQL Server is now available, its a good opportunity to show how to connect to a SQL Server installed on Ubuntu from R, using a JDBC connection. For this tutorial, I am going to assume that you already have R installed. For my purposes,  I am running R on the same Ubuntu machine as the SQL Server. If you need instructions for installing SQL Server on Linux, Microsoft has provided a [write-up already](https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-setup-ubuntu). So now let's get started.
<!-- more -->
##### System preparation

Before you can connect to SQL Server in R, you need to do some system preparation. First download the Microsoft JDBC driver for SQL Server available at this [link](http://www.microsoft.com/downloads/en/details.aspx?FamilyID=a737000d-68d0-4531-b65d-da0f2a735707&displaylang=en). Extract to where ever you like. On my Ubuntu box, I put mine in /opt/jdbc and gave permissions to my user account /group to use it. Now open R (either command line or in RStudio) and install the RJDBC package with the command:

install.packages("RJDBC")

If you get an error like this:

./configure: line 3736: /usr/lib/jvm/default-java/jre/bin/java: No such file or directory

open a terminal prompt and enter this command to install a distribution specific RJava package:

sudo apt-get install r-cran-rjava

Then retry the command: install.packages("RJDBC")

##### Use JDBC to connect to SQL Server

Now that you have the JDBC software installed, lets turn to how to use it. In your R script file or in R Studio, call the JDBC library and define the driver variable to use. The following sample includes the path to where I installed the JDBC jar file. Your path may vary depending on where you installed to above:

library(RJDBC)
driver <- JDBC("com.microsoft.sqlserver.jdbc.SQLServerDriver", "/opt/jdbc/sqljdbc4.jar") 

Next you need to define your connection variable. Replace localhost with your server name if SQL Server is running somewhere else than your R machine. Substitute an appropriate user name and password for those values as well. In this instance we are assuming that we are using a local SQL account and not an Active Directory account to connect to SQL Server. The result should look like this:

connect <- dbConnect(driver, "jdbc:sqlserver://localhost", "username","password")

If you get an error like this:

Error in .jcall(drv@jdrv, "Ljava/sql/Connection;", "connect", as.character(url)\[1\], :
 com.microsoft.sqlserver.jdbc.SQLServerException: The TCP/IP connection to the host localhost, port 1433 has failed. Error: "Connection refused (Connection refused).

Verify the connection properties, check that an instance of SQL Server is running on the host and accepting TCP/IP connections at the port, and that no firewall is blocking TCP connections to the port." Check to make sure the SQL Service is running on your Linux machine, and that the firewall port is not being blocked (by default its 1433). Next define a query variable to return the names of all of the databases on your server:

query <- 'SELECT name, database\_id, create\_date FROM sys.databases ;'

Finally run your query and store the results:

QueryResults <- dbGetQuery(connect, query)

To show the results just call the query results variable:

QueryResults

The results will look like this: [![](http://edpflager.com/wp-content/uploads/2017/04/query-300x118.png)](http://edpflager.com/?attachment_id=3551#main)         FInally if you are finished with querying your database its considered good practice to close your connection. You can do that with the RJDBC package like this:

 dbDisconnect(connect)

That's it!