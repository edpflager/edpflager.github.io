---
title: Create a Pentaho Kettle Repository on SQL Server
tags:
  - cookbook
  - ETL
  - guides
  - How-to
  - howto
  - install
  - kettle
  - SQL Server
  - SysAdmin
  - technical
  - Windows
id: '3492'
categories:
  - - Blog
  - - Business Intelligence
  - - Pentaho
comments: false
date: 2017-01-31 15:43:29
---

[![](http://edpflager.com/wp-content/uploads/2017/01/black-tea-kettle-pv-300x225.jpg)](http://edpflager.com/?attachment_id=3516#main)As I have stated previously when creating ETL workflows, its useful to store the information in a database repository, rather than as individual files on your workstation. This allows multiple users to have access to the information (why recreate the wheel?),  it allows you to pull it into your jobs quickly and easily, and you can back it up quickly and restore it if necessary. With the community version 7.0 of PENTAHO® DATA INTEGRATION (PDI), I am happy to report that you can finally create a repository for your ETL code on Microsoft SQL Server. Previously, you could [setup a repository on MySQL](http://edpflager.com/?p=1622) or PostgreSQL with the community edition but there were compatibility problems with the code that Kettle used that didn't work with SQL Server. After downloading the latest version I was attempting to make a connection to SQL Server, and decided to test setting up a repository again. I am happy to say it works so the remainder of this article will walk through the process of setting up a Pentaho repository on SQL Server 2016 from a Windows 10 machine. Prerequisites:

*   Download the [jTDS open source SQL Server JDBC driver](http://jtds.sourceforge.net/). Extract the ZIP file, and copy the jtds-1.3.1.jar file from your download and save it into the data-integrationlib folder of your Pentaho application. Although Microsoft provides a JDBC driver, it did not work for me.
*   Create an empty database on your Microsoft SQL Server. I created one called "PentahoRepository"
*   Setup a SQL Server user account (not an Active Directory account) on your database server and give the account  DBO (owner) permissions on the database. Using a DDLADMIN level does not work. I created my account and called it "repository". I also set the default database for this account to the new database.

Now that we have our prerequisites setup, we can start the PDI client.
<!-- more -->
1.  Within the PDI client, look at the upper right side of the screen for the work CONNECT. Click on it. [![](http://edpflager.com/wp-content/uploads/2017/01/connect-300x86.png)](http://edpflager.com/?attachment_id=3503#main)
2.  The New Repository window will appear. Under the graphic that is in the middle of the screen, click the Other Repositories link.
3.  The Other Repositories window will appear. Click the top option - Database Repository to select it it, and then click the Get Started button at the bottom.[![](http://edpflager.com/wp-content/uploads/2017/01/databaserepository-300x300.png)](http://edpflager.com/?attachment_id=3510#main)
4.  You'll now see the Connection Details window. Supply a Display Name in the appropriate field, and if you would like a description fill one in that field as well. To have the client open the connection to the repository when you start every time, check the box labeled Launch Connection on startup. At this point, you need to make a connection to the SQL Server. Click the Database Connection field that currently is labeled _None._ 
5.  [![](http://edpflager.com/wp-content/uploads/2017/01/connection-300x300.png)](http://edpflager.com/?attachment_id=3504#main)The window will update and show you that you currently do not have a database connection for this repository. Let's fix that.  Click the Create New Connection button.
6.  [![](http://edpflager.com/wp-content/uploads/2017/01/selectconnection-300x300.png)](http://edpflager.com/?attachment_id=3514#main)The Database Connection window will appear. Enter a name for your connection at the top. In the Connection Type list, scroll up and select MS SQL Server. In the Access list, make sure JDBC is selected. In the host name, enter the host name of your SQL Server or the IP address. In the database name field, enter the database you created previously - "PentahoRepository" in my case. Instance name can be left blank if your SQL Server was installed without one, otherwise supply the appropriate text. The default port for SQL Server is 1433 and will already be filled in. Change it if your DBA informs you that they setup SQL Server with a different port. Finally enter the user name - "repository"  and password that were created earlier on your SQL Server. Finally click the TEST button at the bottom. If everything is working, you will see a small success window appear.
7.  Click OK in the Database Connection Test window, and then OK in the Database Connection window.[![](http://edpflager.com/wp-content/uploads/2017/01/connectiondefined-300x259.png)](http://edpflager.com/?attachment_id=3505#main)
8.  The New Repository Connection window will reappear, this time with the connection you just set up listed. Click the BACK button.
9.  The Connection Details window will reappear. The Database Connection field should now have your newly created Connection entered. Click the FINISH button. An Almost Finished message will display for a short time. Then a Congratulations message will appear, telling you that your connection has been created and is ready to use. (If you examine the database in SQL Server Management Studio you'll see a number of tables have been added to the database.)[![](http://edpflager.com/wp-content/uploads/2017/01/congrats-300x300.png)](http://edpflager.com/?attachment_id=3502#main)
10.  Click CONNECT NOW. The screen updates again and a login screen will appear for your Repository. The user account will already be filled in: admin. For the initial password, enter:admin and click the CONNECT button.[![](http://edpflager.com/wp-content/uploads/2017/01/login-300x300.png)](http://edpflager.com/?attachment_id=3509#main)
11.  You'll see a brief message that the system is connection, and then the window will disappear, leaving you back at the PDI GUI screen.  The upper right of the screen will now indicate that you are connected to your repository.[![](http://edpflager.com/wp-content/uploads/2017/01/pentahoconnected-300x51.png)](http://edpflager.com/?attachment_id=3512#main)
12.  At this point you can proceed to use your repository, but its a good idea to change the Admin password, and add a user for yourself to the repository.
13.  Click on TOOLS in the PDI menu bar, go down to Repository, and over to Explore. Click on it.[![](http://edpflager.com/wp-content/uploads/2017/01/respository-explore-300x115.png)](http://edpflager.com/?attachment_id=3513#main)
14.  The Repository Explorer window will appear. Click on the security tab to see the defined users.
15.  Click on ADMIN in the Available user list on the left to highlight it, then click on  the Pencil icon to edit it. The EDIT USER window appears. User names can't be edited once entered so the user name field is grayed out. In the password field, enter a new password, and then click OK.[![](http://edpflager.com/wp-content/uploads/2017/01/explorer-300x227.png)](http://edpflager.com/?attachment_id=3507#main)
16.  Click on GUEST in the Available user list on the left to highlight it, then click on the X icon to delete it. You'll be prompted to verify the request. Click YES in the security window.[![](http://edpflager.com/wp-content/uploads/2017/01/guest-account-300x227.png)](http://edpflager.com/?attachment_id=3508#main)
17.  Finally, we need to add ourselves as a user of the repository. Click the **+** icon, and the Add User window will appear. Fill in the user name, a password, and you can add a description if you like. Click OK to be returned to the Repository Explorer window. Click CLOSE in the lower right to exit and return to the PDI GUI.[![](http://edpflager.com/wp-content/uploads/2017/01/addnewrepositoryuser-300x228.png)](http://edpflager.com/?attachment_id=3501#main)
18.  Congratulations! You have setup your Pentaho Repository on SQL Server. Now when working in the client, when you save or open jobs and transformations, the client will pull them from the repository database.

PENTAHO DATA INTEGRATION is a registered trademark of Pentaho, Inc.