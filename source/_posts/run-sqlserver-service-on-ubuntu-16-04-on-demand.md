---
title: Run SQLServer service on Ubuntu 16.04 on demand
tags:
  - cookbook
  - guides
  - How-to
  - howto
  - SQL Server
  - SysAdmin
  - technical
  - Ubuntu
id: '3530'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2017-04-25 16:01:18
---

Greetings all! I have been a bit absent from the blog for the past few months due to a variety of external factors, but I **am** endeavoring to be more regular on here. This time around you may be aware that Microsoft recently released a Linux version of SQL Server.  Because I was testing something else, I thought this would be useful to install as well as part of that process.  I won't bother with instructions on how to install it, as Microsoft has provided a decent [write-up already](https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-setup-ubuntu). in my case, I run a variety of different applications on my development system so I don't always want those apps to start when the PC starts up.  I blogged previously on how to [start services on demand in Ubuntu 15.04](http://edpflager.com/?p=2892), so I went back to that post to test it out with the new SQL Server.  Happily, my previous instructions work with this new application! I'll present them here in modified form for SQL Server after the break...
<!-- more -->
##### DISABLE THE MSSQL-SERVER SERVICE

When you install Microsoft SQL Server on Ubuntu, the service is set to start automatically when the system restarts. For a dedicated server, that makes sense, for a variety of reasons. If you are working on a DEV machine like I am, you may not want that. So let's walk through disabling the service, and how to start it when you want it to run.

1.  Open a terminal window, either by pressing CTRL-ALT-T on your keyboard or searching for Terminal in the Unity search interface.
2.  When the terminal window appears, enter this command to see what services are currently running on your PC:
    
    ###### systemctl -t service
    
3.  You should see a fairly lengthy list of services, and more than likely there are more that don't show yet. On the left side is the name of the service (annotated as .service). The three part status follows, and finally a brief description of the service is on the right.[![](http://edpflager.com/wp-content/uploads/2017/04/mssql-300x172.png)](http://edpflager.com/?attachment_id=3534#main)
4.  Press the Enter key to go down one line at a time, or press the space bar to scroll down one screen full at a time. Somewhere in the list you should see mssql-server.service (see the image above).
5.  Once you have verified that the SQL Server service is installed and running, press Ctrl-C to exit back to a terminal prompt.
6.  Enter the following commands to stop and disable the MySQL service:
    
    ###### sudo systemctl stop mssql-server.service sudo systemctl disable mssql-server.service
    
7.  Repeat step 2 from above, and this time the mssql-server.service shouldn't be listed.

##### RUN THE MSSQL-SERVER SERVICE

Once you have disabled the SQL Server service, you can start it up by entering one of these two commands:

######  sudo systemctl start mssql-server.service

 

##### STOP THE SQL SERVER SERVICE

As shown above, to stop the service, enter this command:

**sudo systemctl stop mssql-server.service**