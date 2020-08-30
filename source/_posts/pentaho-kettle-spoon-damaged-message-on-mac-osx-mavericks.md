---
title: Pentaho Kettle Spoon - Damaged message on Mac OSX Mavericks
tags:
  - ETL
  - kettle
  - Mac
  - PDI
id: '1901'
categories:
  - - Blog
  - - Business Intelligence
  - - Pentaho
comments: false
date: 2014-03-26 15:54:02
---

![AppDamaged](http://edpflager.com/wp-content/uploads/2014/03/AppDamaged-300x125.png)Spoon is the graphical front end for designing ETL workflows for Pentaho Data Integration also known as Kettle. The latest community edition (5.01) was released in November of 2013, with versions for Windows, Linux and Macs. On the first two platforms it works very well as soon as you extract the archive, but unfortunately on Mac OS X 10.9 (Mavericks) there are some issues. It is possible to get it run, but its not easy. I'll assume that you have Data Integration downloaded, and extracted on your system and Java 1.6 installed.  The instructions from Pentaho say you can run the Data Integration.app to launch Kettle, but on the systems I've tried this on, I get an error message that the App is damaged. If you are experiencing this, don't click "Move to Trash!" There is a couple of ways to get it working. :) The first method is pretty straightforward.

1.  While you are clicking on the Data Integration application, hold down the Control key on your keyboard. A menu will appear and you can then click on Open near the top. You'll then see an Are You Sure warning window, where you can click Open again. The application will then start. Simple!
<!-- more -->
If you're like me, and sometimes forget about using the Control key, you can bypass that functionality using this handy tutorial.

1.  Go to System Preferences in OSX (hold down Command and Space for a second, then type SYS into the Spotlight window, and hit Enter).
2.  Click on Show All if the entire System Preferences is not shown, then click on Security and Privacy.![SecurityIcon](http://edpflager.com/wp-content/uploads/2014/03/SecurityIcon-300x73.png)
3.  In the Security and Privacy window, make sure you are on the General page, and click on the Lock icon in the bottom left of the screen. Next you'll have to enter your password to unlock the settings.
4.  In the bottom section, under "Allow apps downloaded from:" select the option labeled, Anywhere. (Keep in mind this setting does make your Mac less secure, so use it at your own risk!) You'll get a pop-up message to that effect from Apple as well. Click the "Allow from Anywhere" button. ![SecuritySetting](http://edpflager.com/wp-content/uploads/2014/03/SecuritySetting-300x235.png)
5.  Click the Lock icon again to disallow any other changes, close the System Preferences window, and you are good to go. Start Data Integration up and you should see the normal startup screen.