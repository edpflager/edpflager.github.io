---
title: Use Linux SQL Server with R (ODBC)
tags:
  - cookbook
  - guides
  - How-to
  - howto
  - install
  - SQL Server
  - SysAdmin
  - technical
  - Ubuntu
id: '3555'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2017-05-02 19:49:18
---

[![](http://edpflager.com/wp-content/uploads/2017/04/Rlogo-150x150.png)](http://edpflager.com/?attachment_id=3545#main)This is my second article on using Microsoft's new Linux version of SQL Server with R. This time, I'll cover how to use RODBC to gather data from SQL Server. As a bit of background, over the past few months, I have been working to learn [R, a free software environment](https://www.r-project.org/) for statistical computing. Its been gaining popularity over the past few years, and Microsoft just gave it a huge boost by integrating R into their Power BI visualization software and in the Windows version of SQL Server 2016. Since a good deal of my work involves connecting to Microsoft SQL Servers  its a good opportunity to show how to connect to a SQL Server installed on Ubuntu from R using ODBC. For this tutorial, I am going to assume that you already have R installed. For my purposes,  I am running R on the same Ubuntu machine as the SQL Server. If you need instructions for installing SQL Server on Linux, Microsoft has provided a [write-up already](https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-setup-ubuntu). So now let's get started.
<!-- more -->
##### SYSTEM PREPARATION

Before you can connect to SQL Server in R, you need to do some system preparation. First download the Microsoft 13.1 ODBC driver for SQL Server available at this [link](https://docs.microsoft.com/en-us/sql/connect/odbc/linux/microsoft-odbc-driver-for-sql-server-on-linux). Follow the instructions, and the driver will be installed automatically. On Ubuntu its written to a folder under /opt/microsoft/msodbcsql. With this version the ODBC driver no longer depends on custom packaging for the unixODBC driver manager as was required in prior versions. Now open R (either command line or in RStudio) and install the RODBC package with the command:

install.packages("RODBC")

##### USE ODBC TO CONNECT TO SQL SERVER

Now that you have the ODBC software installed, lets turn to how to use it. There are two methods for ODBC connections to be setup in Linux, one involves defining the connection in an .odbc.ini file. I prefer the second method which involves less setup, but is also less secure because your user name and password are included in the R script. In your R script file or in R Studio, call the RODBC library and define the driver variable to use:

library(RODBC)
driver <- "ODBC Driver 13 for SQL Server" 

Next you need to define your connection variables. Define a database you want to connect to, the hostname of the server (replace localhost with your server name if SQL Server is running somewhere else than your R machine), the port SQL Server is listening on  (1433 by default) and your user name and password. In this instance we are assuming that we are using a local SQL account and not an Active Directory account to connect to SQL Server. The results should look like this:

db <- "master"
host <- "localhost"
port <-"1433"
user <-"username"
pwd <- "password"

Next we need to pull all of those variables together to make our connection string:

conn <- paste("DRIVER=",driver,
              ";Database=",db,
              ";Server=",host,
              ";Port=",port,
              ";PROTOCOL=TCPIP",
              ";UID=", user,
              ";PWD=",pwd,
               sep="")

This results in a connection string like this:

"DRIVER=ODBC Driver 13 for SQL Server;Database=master;Server=localhost;Port=1433;PROTOCOL=TCPIP;UID=username;PWD=password

With our connection built, we can pass it to the odbcDriverConnect function in R to open our connection:

con1 <- odbcDriverConnect(conn)

If all goes well, R will return to the next line prompt. Define a new variable to hold the results of your query and pass the query to the database using the sqlQuery function in R with the connection defined previously:

res <- sqlQuery(con1, 'SELECT name, database\_id, create\_date FROM sys.databases ;')

To show the results just call the query results variable:

res

The results will look like this: [![](http://edpflager.com/wp-content/uploads/2017/04/res-300x97.png)](http://edpflager.com/?attachment_id=3564#main)       Finally if we are finished with using this database, be sure to close the connection:

odbcCloseAll()

That's it!