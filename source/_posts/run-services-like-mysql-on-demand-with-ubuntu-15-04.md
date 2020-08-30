---
title: Run services (like MySQL) on demand with Ubuntu 15.04
tags:
  - guides
  - How-to
  - howto
  - MySQL
  - SysAdmin
  - technical
  - Ubuntu
id: '2892'
categories:
  - - Blog
  - - Linux
comments: false
date: 2015-07-08 17:23:14
---

[![stopstart](http://edpflager.com/wp-content/uploads/2015/07/stopstart-300x285.jpg)](http://edpflager.com/wp-content/uploads/2015/07/stopstart.jpg)This is a followup to an [article](http://edpflager.com/?p=1618) I wrote a couple of years ago where I covered how to start the MySQL server daemon on demand in Ubuntu. With version 15.04, the controller for services in Ubuntu has changed to systemd from upstart. Getting services to start when you want them is still fairly simple, though and I'll illustrate the process by using MySQL as an example. Be careful when disabling services, because you could cause your system to become unstable if you disable the wrong one. For this tutoriaI I assume you have MySQL version 5.6 or higher installed and the server daemon starts up when you boot your system.
<!-- more -->
##### Disable the MySQL Service

Lets walk through disabling that, and how to start MySQL when you want it to run.

1.  Open a terminal window, either by pressing CTRL-ALT-T on your keyboard or searching for Terminal in the Unity search interface.
2.  When the terminal window appears, enter this command to see what services are currently running on your PC:
    
    ###### systemctl -t service
    
3.  You should see a fairly lengthy list of services, and more than likely there are more that don't show yet. On the left side is the name of the service (annotated as .service). The three part status follows, and finally a brief description of the service is on the right.[![services](http://edpflager.com/wp-content/uploads/2015/07/services-300x185.png)](http://edpflager.com/wp-content/uploads/2015/07/services.png)
4.  Press the Enter key to go down one line at a time, or press the space bar to scroll down one screen full at a time. Somewhere in the list you should see mysql.service (see the image above).
5.  Once you have verified that the MySQL service is installed and running, press Ctrl-C to exit back to a terminal prompt.
6.  Enter the following commands to stop and disable the MySQL service:
    
    ###### sudo systemctl stop mysql sudo systemctl disable mysql
    
7.  Repeat step 2 from above, and this time the mysql.service shouldn't be listed.

##### RUN THE MYSQL SERVICE

Once you have disabled the MySQL service, you can start it up by entering one of these two commands:

######    sudo systemctl start mysql sudo service mysql start

##### STOP the MYSQL Service

AS shown above, to stop the service, you can either of these two commands: **sudo systemctl stop mysql    ** **sudo service mysql stop**