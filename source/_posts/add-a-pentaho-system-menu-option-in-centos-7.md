---
title: Add Pentaho to your CentOS Application menu
tags:
  - centos
  - ETL
  - How-to
  - howto
  - install
  - kettle
  - Linux
  - technical
id: '2787'
categories:
  - - Blog
  - - Business Intelligence
  - - Pentaho
comments: false
date: 2015-06-12 19:14:58
---

[![menu](http://edpflager.com/wp-content/uploads/2015/06/menu-180x300.jpg)](http://edpflager.com/wp-content/uploads/2015/06/menu.jpg)A while back, I posted on how to get [Pentaho Data Integration to launch from a desktop shortcut.](http://edpflager.com/?p=2322) Recently, I've installed CentOS7 with Gnome and wanted to install a menu item for PDI. While not too difficult, the process isn't streamlined simple either, so I thought it would be good to document it. Start with the same process I've covered before for setting up a "start-pentaho.sh" bash script. Once you have it created, copy the file (using the root account) to the folder where you installed Pentaho on your system. In my case,  it is under /opt/pentaho/data-integration, so I copied the "start-pentaho.sh" file to the /opt/pentaho folder and renamed it to "start-spoon.sh".
<!-- more -->
Next, with a text editor, create a file called PentahoDataIntegration.desktop in your home folder with the following contents: \[Desktop Entry\] Encoding=UTF-8 Name=PentahoDataIntegration GenericName=PentahoDataIntegration Exec=/opt/pentaho/start-spoon.sh Terminal=true Icon=/opt/pentaho/data-integration/spoon.png Type=Application Save the file. From a terminal, as root, move the .desktop file from your home folder to  /usr/share/applications. Once its in the new location, in your menu bar, click on Applications, and go down to the (new) Other option. In that folder, you will see an entry for Pentaho Data Integration. [![pdi-menu](http://edpflager.com/wp-content/uploads/2015/06/pdi-menu-300x261.png)](http://edpflager.com/wp-content/uploads/2015/06/pdi-menu.png)Click the PDI entry, and a new terminal window will open, and after a few seconds, the PDI splash screen should appear!