---
title: ApacheDS (LDAP) - Part 1
tags:
  - cookbook
  - guides
  - How-to
  - howto
  - install
  - LDAP
  - Mint
  - SysAdmin
id: '3029'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2015-11-19 15:45:15
---

[![directory](http://edpflager.com/wp-content/uploads/2015/11/directory-300x274.jpg)](http://edpflager.com/wp-content/uploads/2015/11/directory.jpg)While working on a recent project, I found the software I was researching had the capability to work with an LDAP server. My experience with LDAP has been pretty limited, mainly using Microsoft's Active Directory for work, so I decided to look into open source alternatives. For the uninitiated, LDAP (Lightweight Directory Access Protocol) and an LDAP server provide a centralized database where an individual's user account and password is stored and then shared between many services on a network. Many other entities on the network (groups, servers, printers, etc) can also be referenced in an LDAP catalog, making discovery and access much easier. When a user logs into a company's network, they are then able to access other network resources (email, other internal applications, file shares, printers), by using the same user name and password. The network administrator only needs to enter the user's information in one location, and all security settings are defined in that location as well. There are a number of Open Source implementations of LDAP, including OpenLDAP, OpenDS/DJ, and ApacheDS.  OpenLDAP is a well documented project, and has been around for a number of years. OpenDS is owned by Sun MIcroSystems and is no longer maintained and was forked to the OpenDJ project.  After researching, I decided for my purposes that the ApacheDS server would best meet my needs, so this article will walk through installing and getting ApacheDS started on a Linux Mint box. This first section should work OK on any Ubuntu/Debian based distribution, and most of this series will work fine on any distribution. YMMV
<!-- more -->
SET YOUR HOSTNAME

1.  As a first step, make sure your Linux Mint box is up to date. From a command prompt, run the below command:
    
    sudo apt-get update
    
2.  Check your hostname, and rename your box as appropriate. To check your current hostname, enter:
    
    hostname
    
3.  To edit the host name, edit the **/etc/hostname** file as root. If you have a local domain for your network, be sure to include it as part of the host name. For example, if your domain is test.com, you might make the host name: **ldap.test.com. ** Open nano to edit the file with this command:
    
    sudo nano /etc/hostname
    
    Save the file and close it when you are complete.

SET A STATIC IP ADDRESS

1.   You now need to add a static IP address to your LDAP system. If your network administrator has provided one for you, use it. Otherwise to get the existing address for your PC, the gateway, the net mask and a lot of other info in the  terminal window, enter the command:
    
    **ifconfig**
    
2.  From the Mint menu, access the Network Connections application, under Preferences. In the network connections window, click on Wired Connection 1, and then click the EDIT button on the right.[![NetworkConnection](http://edpflager.com/wp-content/uploads/2015/11/NetworkConnection-300x243.png)](http://edpflager.com/wp-content/uploads/2015/11/NetworkConnection.png)
3.  A new window will appear with several tabs. Click on the one for IPv4. If the Method here is not set to MANUAL, click  the drop down list and select that option.[![IPv4](http://edpflager.com/wp-content/uploads/2015/11/IPv4-238x300.png)](http://edpflager.com/wp-content/uploads/2015/11/IPv4.png)
4.  Click the NEW button on the right, and enter your IP information. Here is where you will enter the IP address, Net Mask and Gateway information provided either by your network administrator, or from the **ifconfig** command from step 1. **Keep in mind that if you use the already assigned DHCP address, you may encounter issues on your home network if that address is given out to another device by your router or DHCP server. Be sure to exclude it from the range of addresses that are handed out.**
5.  Enter at least one DNS server (generally the first one will be the address of your gateway). You can also use Google's public DNS servers, by adding in **8.8.8.8** and/or **8.8.4.4.** Separate multiple server addresses by using a comma.
6.  Click over to the IPv6 tab. For our purposes, we don't need to set a static address here, so set the Method drop down to IGNORE. Click the SAVE button to the Network Connection window and then click CLOSE in that window.
7.  At this point, you should turn networking off and on using the network icon on your screen to ensure the new settings have taken effect. Click the icon once to bring up a control window. Toggle the switch at the top of the window to the O setting to turn it off (you should get an onscreen notification that you are disconnected). Toggle the switch back to the **\-** setting to reconnect. (you should get another onscreen notification that you are connected).[![restartnetwork](http://edpflager.com/wp-content/uploads/2015/11/restartnetwork.png)](http://edpflager.com/wp-content/uploads/2015/11/restartnetwork.png)
8.  Finally, open a terminal prompt, and attempt to PING a website to make sure everything is working OK. Enter the command:
    
    ping www.linuxmint.com
    
    (or some other website) and hit ENTER. You should receive a number of reply messages that  64 bytes were received from the website in a certain amount of time. Hit CTRL-C to quit the PING application.

EDIT THE LOCAL HOST FILE

1.  The next step is to configure the local  HOST file to show the new server name and IP address for your Mint PC. The **hosts file** is used by the operating system to map host names to IP addresses.
2.  Open a terminal prompt again, and enter this command:
    
     sudo nano /etc/hosts
    
3.  The nano text editor will open and display the contents of your HOSTS file. It should look something like this:
    
    127.0.0.1    localhost
    127.0.1.1    originalname
    
4.  Change the second line to have your static IP address, the new Host Name you set previously, and the full host name with domain. After editing, it should look like:
    
    127.0.0.1         localhost
    10.0.1.22         **ldap       ldap.test.com**
    
5.  Restart (just in case).

CHECK YOUR JAVA VERSION

1.  Open a terminal prompt and enter the command:
    
    java -version
    
    to see if JAVA is installed. By default,  OpenJDK is installed, or you can replace it with the official version of JAVA from Oracle. If you prefer the Oracle version, the Linux Mint website has a good installation [how to](http://community.linuxmint.com/tutorial/view/1091).

FINALLY, INSTALLING APACHE DS

1.  Download the latest version of ApacheDS from the [app website](http://directory.apache.org/apacheds/download/download-linux-deb.html) (either 64-bit or 32-bit depending on your system's architecture).
2.  Open a Terminal and navigate to where you downloaded the DEB package  and run:
    
     sudo dpkg -i apacheds-2.0.0-M20-amd64.deb (substitute the version number you downloaded)
    
3.  After supplying your SUDO password, the software will install very quickly and you will see a message that the reconfigured ureadahead will take effect after the next reboot.
4.  Before you reboot, set the LDAP service to start automatically when you boot the system. Open a terminal prompt, and execute the following commands:
    
    $ cd /etc/init.d
    $ sudo update\-rc.d apacheds\-2.0.0\-M20\-default defaults
    
5.  You'll see several lines of output indicating that ApacheDS has been added to various runlevels on your system.  Go ahead and reboot now.
6.  That's it! ApacheDS should now be up and running on your system.

In Part 2, I'll cover installing the ApacheDS Studio and configuring it to connect to your LDAP catalog, as well as entering your first entity in the catalog.