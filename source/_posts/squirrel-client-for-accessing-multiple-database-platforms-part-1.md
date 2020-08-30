---
title: SQuirreL SQL Client for accessing different databases - Part 1
tags:
  - centos
  - ETL
  - guides
  - How-to
  - howto
  - install
  - Mac
  - MySQL
  - SysAdmin
  - technical
id: '2778'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2015-06-21 15:29:54
---

[![squirrel](http://edpflager.com/wp-content/uploads/2015/06/squirrel-164x300.jpg)](http://edpflager.com/wp-content/uploads/2015/06/squirrel.jpg)Its been my experience that if you work on ETL projects, you eventually accumulate client software for a number of database systems on your development PC. The reason is pretty straightforward - you need to be able to access the systems you are working with to determine data types, schema structures, and occasionally to check that a User account and Password you have been given actually works. One problem I've run into though is that not all operating systems are supported by different database vendors with their tools. While Windows has the largest installation base, Mac OS X, and Linux also are used for ETL development  but Microsoft's SQL Server management tool will only work on Windows machines. Apple's FileMaker software is similar, running on Mac OS X and Windows, but not Linux (since version 7). The examples go on and on. Also, because each tool is laid out differently, it can be difficult to find what you need quickly when you only work infrequently on a specific platform. Often times remembering where I need to go in a specific tool will take me longer than getting the actual information I was looking for. All of this leads to the point of this post - using a free open source product call SQuirreL SQL Client to access multiple database platforms via one application regardless of whether you are running Windows, Mac OS X or any of a large variety of Linux distributions.
<!-- more -->
##### **Installation of SQuirreL SQL Client**

\[caption id="attachment\_2807" align="aligncenter" width="300"\][![squirrelscreen](http://edpflager.com/wp-content/uploads/2015/06/squirrelscreen-300x164.png)](http://edpflager.com/wp-content/uploads/2015/06/squirrelscreen.png) SQuirreL SQL Client connected to a test server running Microsoft's SQL Server 2012.\[/caption\]

1.  To get started, make sure you have Oracle's JAVA client installed on your system. If you are running Linux, I've found that OpenJDK doesn't work right with this, so be sure you have the correct software. On most operating systems, you can open a terminal prompt and enter the following to see if you have it installed: **JAVA -version.**
2.  Next go to this website: [http://www.squirrelsql.org/](http://www.squirrelsql.org/) and click the "Download and Installation" link in the toolbar and then click the link for your operating system. A small JAR file will be downloaded. If you receive a message that these types of files can be harmful to your system, click the option that lets you down the file anyway.
3.  On Windows or Linux, open a terminal prompt and navigate to where you downloaded the installation file. Enter the command: **java -jar squirrel-sql-3.6-standard.jar<ENTER> ** to run the installation routine.
4.  On Mac OS X, open the folder where you downloaded the file in Finder, and double click it.
5.  An lzpack window will appear. Click NEXT on the welcome screen, and NEXT on the please read windows. You can decide where to install the application on the following screen (the default location is usually OK unless you have a compelling reason for installing it elsewhere). Note on Linux,  if you run the install as a normal user, the installation will default to a folder in the user's HOME folder. Installed as root, it defaults to /usr/local.
6.  On the following screen, Choose the add-ins you would like to use such as any DB platforms you anticipate connecting to, the optional plugins for Smart Tools and SQL (located between PostgreSQL and Sybase in the list), and any translations plugins you may need. Some additional plugins you may want to select are:
    *   Session Scripts - run SQL when opening an session
    *   SQL Parametrisation - put variables into your SQL statements
    *   SQL Replace - place environment variables into your SQL statements
    *   SQL Validator - validate SQL against the ISO SQL-99 standard
7.  Click Next to watch the installation progress, and then DONE when it finishes. Finally, you have the option to create shortcuts and allow all users to access the application. Select specific options according to your needs, and then finish with DONE.

 The program is now installed. To run it in Linux, run the squirrel-sql.sh bash file, in Windows, execute the squirrel-sql.bat file, and on a Mac, run the SquirreLSQL.app file.

##### Configuration of SQuirreL SQL Client

At this point, you can run SquirreL but it won't be able to connect to much. In order to access different DB platforms, you'll need to download the appropriate JDBC drivers for the databases you want to use. Check with your database vendor for web locations where the driver can be found. Once you have the JDBC JAR files, just copy them to the /squirrel-sql-3.6/lib folder.  On Linux and Windows, the installation of the drivers is fairly straightforward. On Mac OS X, you'll need to open up the finder folder where the SQuirreL application has been installed. If you chose the default location, hit Shift-Command-G on your keyboard to open the Go to Folder window, and paste this in: /Applications/SquirreLSQL/Contents/Resources/Java/lib. Hit Enter and after a second or two the Lib folder will appear.

1.  Once you have the JDBC JAR file installed, restart SQuirrel, and click the DRIVERS tab on the left side. The installed drivers should now appear with a check mark. If there is a red circle with an X, then the driver is not installed.
2.  Click on the Aliases tab to add a connection to database.
3.  Click the Plus icon to add a new alias. Enter a name in the appropriate box. Click the Driver drop down and choose the appropriate checked driver.
4.  Update the URL to the appropriate one for your database. For example for a locally installed MySQL database, enter: **jdbc:mysql://localhost:3306** or for a specific database: ** jdbc:mysql://localhost:3306/databasename**
5.  Enter a user name in the space for it, and LEAVE THE PASSWORD FIELD EMPTY. If you enter one in, and save it, it will be saved in clear text. (That's a BAD security breach.) Click OK to exit, and a new entry with the name you specified will appear in the Aliases panel.
6.  Double click the entry you created to initiate a connection. A Connect To box will appear with the user name filled in. Enter your password, and click the Connect button.
7.  If all goes well, you'll see a new window appear on the canvas with an object tree on the left. At the top, the catalog drop down will show the currently connected schema or database. You can only work in the database or schema that is selected here.

Next time we'll look at how to navigate around in SquirreL SQL Client and access multiple DB platforms at the same time!