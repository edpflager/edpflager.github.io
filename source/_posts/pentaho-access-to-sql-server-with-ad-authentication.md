---
title: Pentaho access to SQL Server with AD Authentication
tags:
  - ETL
  - How-to
  - PDI
  - technical
id: '2334'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
  - - Pentaho
comments: false
date: 2014-08-04 15:24:20
---

[![keys3](http://edpflager.com/wp-content/uploads/2014/08/keys3-200x300.jpg)](http://edpflager.com/wp-content/uploads/2014/08/keys3.jpg)Back in June, I posted on how to [Use SQL Server with Pentaho Data Integrator](http://edpflager.com/?p=2130). That article covered using Microsoft SQL Server (MSSQL) authentication to connect to your MSSQL databases. This time around, we'll look at the other type of authentication that you can encounter when working with MSSQL - Active Directory authentication (AD for short). If you are not familiar with AD, its a centralized authentication mechanism allowing access to the various hardware and services in the network. By centralizing the authentication process, the same user account can be used to access multiple resources, and it eliminates some of the setup needed to enable those users on various systems. Most DBA's prefer to use AD authentication for those reasons, and if you will be using PDI to access multiple MSSQL systems, you'll probably want to become familiar with setting it up.
<!-- more -->
1.  Although Microsoft provides their own JDBC driver, which I covered in the previous post, this time around we will be using an open source driver called jTDS. You can download the driver from SourceForge using this [link](http://sourceforge.net/projects/jtds/). Download the most current version (at this writing it was version 1.3.1).
2.  Extract the archive file and open it. Copy the jtds-1.3.1.jar file to the Pentaho lib folder on your system. On my system that is currently /opt/pentaho.data-integration/lib.
3.  In the folder where you extract the archive, locate the subfolder matching your systems architecture (x64, x86 or IA64). Open it, and open the SSO subfolder.
4.  Copy the ntlmauth.dll file to a folder on your path. (On CentOS from a command prompt enter: ECHO $PATH$ to see the current path). On my system, I copied the file (as root) to the /usr/local/bin folder.
5.  Open the Pentaho DI GUI (aka Spoon) and start a new job. Click on the VIEW tab in the Explorer panel.
6.  Right click on Database Connections, and choose NEW to open the Database Connection window.
7.  Enter a name in the Connection Name box to identify it.
8.  Scroll down in Connection Type and choose MS SQL Server.
9.  In the Access panel, make sure Native (JDBC) is selected.
10.  In the Settings panel, enter your server's hostname or IP address, the database you want to connect to, the port SQL Server is using (by default its 1433), and the user name and password in the appropriate fields. You can leave Instance Name empty unless your DBA tells you the server is using a named instance. It should look something like this:[![Screenshot-Database Connection -1](http://edpflager.com/wp-content/uploads/2014/08/Screenshot-Database-Connection-1-300x278.png)](http://edpflager.com/wp-content/uploads/2014/08/Screenshot-Database-Connection-1.png)
11.  In the left most panel, select Options. The right panel will refresh, and will probably only have one value entered: "instance". Leave the value as is.
12.  Add a parameter called "integratedSecurity" (watch the text case), and set the value to true.
13.  Add another parameter called "domain" and set the value to your network's domain name. (You can use the full domain name or the shorthand one).[![Screenshot-Database Connection -2](http://edpflager.com/wp-content/uploads/2014/08/Screenshot-Database-Connection-2-300x278.png)](http://edpflager.com/wp-content/uploads/2014/08/Screenshot-Database-Connection-2.png)
14.  Click the TEST button at the bottom of the screen, and you should be rewarded with a successful connection window. Click OK and you are done.

[![Screenshot-Database Connection](http://edpflager.com/wp-content/uploads/2014/08/Screenshot-Database-Connection--300x164.png)](http://edpflager.com/wp-content/uploads/2014/08/Screenshot-Database-Connection-.png) Pentaho is a trademark of Pentaho, Inc.