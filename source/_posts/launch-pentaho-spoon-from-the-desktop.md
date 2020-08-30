---
title: Launch Pentaho Spoon from the Desktop
tags:
  - ETL
  - How-to
  - kettle
  - PDI
  - technical
id: '2322'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
  - - Pentaho
comments: false
date: 2014-07-29 16:08:47
---

[![launch](http://edpflager.com/wp-content/uploads/2014/07/launch-300x201.jpg)](http://edpflager.com/wp-content/uploads/2014/07/launch.jpg)Coming from a Windows/Mac background, I got in the habit of having shortcuts to applications I use frequently on my desktop or in a task bar. I've continued that practice when switching to Linux as well. Unfortunately, Pentaho Spoon - the GUI tool for designing transformations and jobs for Pentaho Data Integration (aka Kettle) is started from a command line. When I tried to create a desktop launcher on my CentOS laptop, but that only resulted in an error message:

###### Unable to access jarfile launcher/pentaho-launcher-5.1.0.0-752.jar

I searched on the Internet, and apparently this was a common question, so I decided to come up with a quick and easy solution. Here it is:
<!-- more -->
1.  In your home folder, create a text file. I called mine "start-pentaho.sh", but feel free to call it whatever you like, as long as you have the .sh extension.
2.  Edit the file and add these three lines: #!/bin/sh cd <path to where you extracted PDI>. On my laptop, its: cd /opt/pentaho/data-integration ./spoon.sh
3.  Save the file.
4.  Open up the file browser, and go to your home folder. Locate the "start-pentaho.sh" file you just created, and right click on it and choose properties from the menu.
5.  In the Properties window that appears, switch to the Permissions tab. For the owner and group, make sure the Access drop down is set to Read and Write and for others it is set to Read-only. Finally at the bottom of the window, check the box next to "Allow executing file as program".[![properties](http://edpflager.com/wp-content/uploads/2014/07/properties-282x300.png)](http://edpflager.com/wp-content/uploads/2014/07/properties.png)
6.  Click Close to save your changes and return to the desktop.
7.  On your desktop, right click and choose: Create Launcher
8.  In the type field, choose "Application in Terminal". Enter whatever you would like in the Name field. Finally, for the Command field, you can click Browse to where you created your startup file and choose it, or enter the path manually. When you are finished the results should look similar to this:![launcher](http://edpflager.com/wp-content/uploads/2014/07/launcher-300x130.png)
9.  Before you click OK, you can click the spring icon in the upper left corner, and navigate to the data-integration folder on your system to substitute a Pentaho icon for the spring.
10.  Once you have changed the icon, click OK, and you should now have a desktop shortcut to start Pentaho.
11.  Double click the window and a terminal will open with the first line of the script showing: cd /opt/pentaho/data-integration. After a few seconds, the Pentaho splash screen will appear, and then the application will load up after that!

**Update:** If you are running Linux Mint Mate (have not tried this on Cinnamon), you need to install Gnome-terminal for it to work. Launch the Software Manager application, search for gnome-terminal and install it. Then the above will work. PENTAHO is a trademark of Penatho, Inc.