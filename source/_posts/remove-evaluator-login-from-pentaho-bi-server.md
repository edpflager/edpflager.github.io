---
title: Remove evaluator login from Pentaho BI server
tags:
  - guides
  - How-to
  - howto
id: '3020'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
  - - Pentaho
comments: false
date: 2015-11-11 13:13:45
---

[![evaluate](http://edpflager.com/wp-content/uploads/2015/11/evaluate-173x300.jpg)](http://edpflager.com/wp-content/uploads/2015/11/evaluate.jpg)When you first install the Pentaho BI server, the login screen includes an option to **Login as an Evaluator**, either as an Administrator (Admin) or a Power User (Suzy). While this is handy if you just want to check the software out, its a huge security hole if you plan to move to production mode. The good news is that removing that functionality involves editing one configuration file to change a couple of settings. Open a terminal and navigate to where the BI-Server was installed. On my system that is **/opt/pentaho/biserver-ce**. Drill down into  the pentaho-solutions folder, and then to system folder. Using a text editor, open the pentaho.xml file.
<!-- more -->
Look for the option **login-show-user-list**. It should look like this

\-->
<login-show-users-list>true</login-show-users-list>
<!--

Change the value "true"  to "false". Now look for the option node <login-show-sample-users-hint> which should follow the first one.

\-->
 <login-show-sample-users-hint>true</login-show-sample-users-hint>
<!--

Again the value should by default be "true". Change it to "false". Save the file and exit. Restart the BI-Service daemon, and the option to **Login as an Evaluator** should no longer appear. Be sure to change the default Admin password, and remove the Suzy account to make your system more secure.