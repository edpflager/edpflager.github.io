---
title: ApacheDS (LDAP) - Part 2
tags:
  - cookbook
  - guides
  - How-to
  - howto
  - install
  - LDAP
  - Mint
  - SysAdmin
  - technical
id: '3055'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2015-11-23 02:26:30
---

In the first part of this series, I walked through installing Apache Directory Server (ApacheDS), an LDAP server, on Linux Mint. In this part, we'll cover how to install and configure the management tool for ApacheDS, called Apache Directory Studio. Download and extract Apache Directory Studio from the [Apache website](http://directory.apache.org/studio/download/download-linux.html). If you are the only person on your system who will be using it, its OK to extract it to your Home folder. For shared systems, you may wish to extract it to the /opt folder, create a group with access to it, and make each user a member of that group. Apache Directory Studio is built on the Eclipse framework, so if you are familiar with Eclipse it will be fairly easy to navigate.
<!-- more -->
1.  Start the Apache Directory Studio application by double clicking it in Nemo (file manager for Mint), or from a Terminal prompt, type: ./ApacheDirectoryStudio followed by <ENTER>
2.  When APS first starts, you'll see a Welcome window.![DirStudio1stStart](http://edpflager.com/wp-content/uploads/2015/11/DirStudio1stStart-300x202.png)
3.  Go ahead and close that by clicking the X on the Welcome tab, and you will see the LDAP perspective.[![LDAP Perspective](http://edpflager.com/wp-content/uploads/2015/11/LDAP-Perspective-300x194.png)](http://edpflager.com/wp-content/uploads/2015/11/LDAP-Perspective.png)
4.  In the lower left side of the main window, you will see the Connections panel. Hover your cursor over the LDAP icon and you'll see a tool tip that says "New Connection".[![NewConnection](http://edpflager.com/wp-content/uploads/2015/11/NewConnection1-300x246.png)](http://edpflager.com/wp-content/uploads/2015/11/NewConnection1.png)
5.  Click on the LDAP icon and a New Connection window will open. On the first page, Network Parameter, for the **Connection Name**, provide a user friendly name. For the **Hostname** field, enter the server name for the LDAP box. ApacheDS default to using **port** 10389 (the common port for LDAP is 389). For now, you can use "No encryption" for the **Encryption Method**. Make sure the **Provider** drop down has "Apache Directory LDAP Client API" selected. Click the **Check Network Parameter** button and a test connection will be attempted. If everything is OK, you'll see a popup window letting you know. Click OK to close that popup, and then click NEXT on the Network Parameter window.![LDAPConnectionWindowFilledOut](http://edpflager.com/wp-content/uploads/2015/11/LDAPConnectionWindowFilledOut-250x300.png)
6.  You'll now see the **Authentication** window where you can enter your credentials for accessing the Apache DS server. Leave the **Authentication Method** set to "Simple Authentication".  In the **Bind DN or user** field, enter "uid=admin; ou=system".This will let the server know that you want to login with user ID(uid): **admin** from the **system** organization unit (ou). Click the SAVE password checkbox, and then click the Check Authentication button. If you have entered everything correctly, you'll see a message that authentication was successful. Click OK to dismiss the message, and then NEXT on the Authentication window.![ApacheDSConnectionAuthentication](http://edpflager.com/wp-content/uploads/2015/11/ApacheDSConnectionAuthentication-249x300.png)
7.  In the **Browser Options** window, leave the defaults as they are, and click the **Fetch Base DNS** button. If everything is working well, one DNS entry will be retrieved and you'll see a detail message showing what was retrieved. Click OK to dismiss it and then NEXT to advance to the next screen.![BrowserOptions](http://edpflager.com/wp-content/uploads/2015/11/BrowserOptions1-300x214.png)
8.  The last window you see will be the **Edit Options** window. You can accept the values here, and click the FINISH button to save your work.![ApacheDSStudio Edit Options](http://edpflager.com/wp-content/uploads/2015/11/ApacheDSStudio-Edit-Options-247x300.png)
9.  The LDAP perspective will update, and you'll see the Connection tab has a new entry and the LDAP browser will show an DIT tree. Congratulations! You have configured Apache Directory Studio to connect to your server.

![Screenshot from 2015-11-22 20:42:21](http://edpflager.com/wp-content/uploads/2015/11/Screenshot-from-2015-11-22-204221-117x300.png)

In the third part of this series, I'll cover some basic editing tasks for your new LDAP system.