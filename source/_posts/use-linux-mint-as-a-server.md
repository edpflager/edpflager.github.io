---
title: Use Linux Mint as a Server
tags:
  - guides
  - How-to
  - howto
  - Mint
  - SysAdmin
  - technical
id: '3173'
categories:
  - - Blog
  - - Linux
comments: false
date: 2015-12-14 19:53:39
---

[![mortar-mint](http://edpflager.com/wp-content/uploads/2015/12/mortar-mint-300x272.png)](http://edpflager.com/?attachment_id=3193#main)Linux Mint has been consistently at the top of the DistroWatch top ten list for more than a year due I am sure to its ease of use and solid performance. As a general purpose distribution, Mint is offered with several different desktop environments, but interestingly, no separate server distribution. However, it is possible to configure Mint as a server, and this article covers the basic setup I follow to configure a Mint system for **development server** purposes. As a warning however, be sure to lock your system down with appropriate security precautions before using one as a production server. Below are the standard steps I use while configuring Mint for **development systems**. Because I follow these steps pretty regularly, I have pulled them out as a separate article, and will link back to this as necessary for any future tutorials.
<!-- more -->
SET YOUR HOST NAME

1.  After installing MINT from your ISO boot media, make sure your Linux Mint box is up to date. From a command prompt, run the below command:
    
    sudo apt-get update
    
2.  During installation you can provide a hostname, but I still prefer to validate it after the system is up to check, and rename your box as appropriate. To check your current hostname, enter:
    
    hostname
    
3.  To edit the host name, edit the **/etc/hostname** file as root. If you have a local domain for your network, be sure to include it as part of the host name. For example, if your domain is test.com, you might make the host name: **ldap.test.com.** Open nano to edit the file with this command:
    
    sudo nano /etc/hostname
    
    Save the file and close it when you are complete.

SET A STATIC IP ADDRESS

1.   You now need to add a static IP address to your LDAP system. If your network administrator has provided one for you, use it. Otherwise to get the existing address for your PC, the gateway, the net mask and a lot of other info in the  terminal window, enter the command:
    
    **ifconfig**
    
2.  From the Mint menu, access the Network Connections application, under Preferences. In the network connections window, click on Wired Connection 1, and then click the EDIT button on the right.![](http://edpflager.com/wp-content/uploads/2015/11/NetworkConnection.png)
3.  A new window will appear with several tabs. Click on the one for IPv4. If the Method here is not set to MANUAL, click  the drop down list and select that option.![](http://edpflager.com/wp-content/uploads/2015/11/IPv4.png)
4.  Click the NEW button on the right, and enter your IP information. Here is where you will enter the IP address, Net Mask and Gateway information provided either by your network administrator, or from the **ifconfig** command from step 1. **Keep in mind that if you use the already assigned DHCP address, you may encounter issues on your home network if that address is given out to another device by your router or DHCP server. Be sure to exclude it from the range of addresses that are handed out.**
5.  Enter at least one DNS server (generally the first one will be the address of your gateway). You can also use Google’s public DNS servers, by adding in **8.8.8.8** and/or **8.8.4.4.**Separate multiple server addresses by using a comma.
6.  Click over to the IPv6 tab. For our purposes, we don’t need to set a static address here, so set the Method drop down to IGNORE. Click the SAVE button to the Network Connection window and then click CLOSE in that window.
7.  At this point, you should turn networking off and on using the network icon on your screen to ensure the new settings have taken effect. Click the icon once to bring up a control window. Toggle the switch at the top of the window to the O setting to turn it off (you should get an onscreen notification that you are disconnected). Toggle the switch back to the **–** setting to reconnect. (you should get another onscreen notification that you are connected).![](http://edpflager.com/wp-content/uploads/2015/11/restartnetwork.png)
8.  Finally, open a terminal prompt, and attempt to PING a website to make sure everything is working OK. Enter the command:
    
    ping www.linuxmint.com
    
    (or some other website) and hit ENTER. You should receive a number of reply messages that  64 bytes were received from the website in a certain amount of time. Hit CTRL-C to quit the PING application.

##### EDIT THE LOCAL HOST FILE

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

##### CHECK YOUR JAVA VERSION

1.  Open a terminal prompt and enter the command:
    
    java -version
    
    By default,  OpenJDK is installed in Mint, but you can replace it with the official version of JAVA from Oracle. If you prefer the Oracle version, the Linux Mint website has a good installation [how to](http://community.linuxmint.com/tutorial/view/1091). Or since Linux Mint is a derivative of Ubuntu, you can use [this tutorial](https://www.howtodojo.com/2015/07/how-to-install-jdk-8-on-ubuntu-15-04/) from www.howtodojo.com which is quite a bit simpler (substitute Java7 for Java8 if you need the older version).

##### INSTALL OPENSSH-SERVER

1.  The last thing I install on my new server is SSH. This allows me to open a secure terminal connection to the server from whatever system I am working on. Again from a terminal prompt, enter this command:
    
    sudo apt-get install openssh-server
    
2.  Then from another server, I open a terminal prompt and enter the command:
    
    ssh <ip-address of server> -l <user name>
    
3.  I'll be prompted to accept that the authenticity of the host I am trying to connect to cannot be established, and am I sure I want to connect. Enter yes and press ENTER, and then I am prompted to enter my password. Enter the password for the account being used to connect and press ENTER again.  [![ssh-login1](http://edpflager.com/wp-content/uploads/2015/12/ssh-login1-300x201.png)](http://edpflager.com/?attachment_id=3195#main)
4.  If all is good, the terminal prompt will change to indicate you are connected to the new server.

At this point, my basic server is setup and configured, and I can continue on with whatever applications I need to install.