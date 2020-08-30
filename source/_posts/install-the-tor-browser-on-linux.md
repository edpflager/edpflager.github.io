---
title: Install the Tor Browser on Linux
tags:
  - guides
  - How-to
  - howto
  - install
  - Mint
  - SysAdmin
  - technical
id: '3220'
categories:
  - - Blog
  - - Linux
comments: false
date: 2016-01-10 07:34:59
---

[![DCF 1.0](http://edpflager.com/wp-content/uploads/2016/01/onions-1463141-225x300.jpg)](http://edpflager.com/?attachment_id=3228#main) The TOR project was started by United States Naval Research Lab employees in the early part of the 21st century as a way to protect intelligence communications  online. It was open sourced in 2004, and continues to be supported by the Electronic Frontier Foundation. As privacy as continued to decline on the Internet, interest in the Tor Browser and Tor project has increased. For more information on TOR see this recent [article at Salon.com](http://www.salon.com/2016/01/08/this_is_the_web_browser_you_should_be_using_if_you_care_at_all_about_security_partner/). I would like to note that I don't support or condone the use of TOR for illicit or illegal purposes, but for those who feel that their privacy is important , I am providing these instructions for how to install the Tor Browser on Linux. These instructions should work for most Linux distros.
<!-- more -->
1.  Download the appropriate file for your O/S from the [TorBrowser website.](https://www.torproject.org/projects/torbrowser.html.en#downloads)  In my case, I downloaded the Linux 5.0.7 Linux 64 bit version for English to my Downloads folder.
2.  Extract the file at the terminal with this command:
    
    tar -xvf tor-browser-linux64-5.0.7\_en-US.tar.xz
    
3.  Move the tor folder to the /opt folder with this command:
    
    sudo mv tor-browser\_en-US /opt
    
4.  Switch to the /opt/tor-browser\_en-US and check the file permissions on the TOR folder. Make sure everything is owned by your user name:
    
    ls -la tor\*
    
5.  In the tor-browser\_en-US folder, execute the start-tor-browser.desktop file. This file is designed to update itself with the location of where it is installed.:
    
    ./start-tor-browser.desktop
    
6.  You'll see a response Launching './Browser/start-tor-browser --detach'...
7.  The first time you start TOR you will be prompted to select how you are connecting to the Internet. Unless you know for sure you are using a proxy, select the default option of connecting directly to the Tor network. You can always change this option later by clicking the Open Setting button on the Tor browser startup window.[![tor startup](http://edpflager.com/wp-content/uploads/2016/01/tor-startup-242x300.png)](http://edpflager.com/?attachment_id=3225#main)
8.  Tor will connect to the Internet, and the browser will open.[![torconfig](http://edpflager.com/wp-content/uploads/2016/01/torconfig-300x193.png)](http://edpflager.com/?attachment_id=3226#main)
9.  Close the Tor Browser momentarily and copy the start-tor-browser.desktop file to your personal desktop:
    
    cp ./start-tor-browser.desktop ~/Desktop
    
10.  Now you can start the program up from a desktop shortcut. If you would prefer to add the application shortcut to your menu, for Linux Mint there is a good tutorial [here](http://community.linuxmint.com/tutorial/view/1504). For other distros, please check Google.