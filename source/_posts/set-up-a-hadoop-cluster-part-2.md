---
title: Set Up a Hadoop Cluster - Part 2
tags:
  - CDH
  - Hadoop
  - HortonWorks
id: '1721'
categories:
  - - Big Data
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2014-01-25 09:40:49
---

[![StaticIP](http://edpflager.com/wp-content/uploads/2014/01/StaticIP-237x300.png)](http://edpflager.com/wp-content/uploads/2014/01/StaticIP.png)In [part 1 of this series](http://edpflager.com/?p=1714 "Set up a Hadoop Cluster – Part 1"), I talked about starting a project to set up a cluster of inexpensive refurbished PCs (cheap) to act as a test Hadoop cluster, and some of the issues I encountered in getting the operating system installed. With this part, I'll talk a little bit about configuring these workstations to get them setup for Hadoop. (And I did want to add, I introduced a fourth PC into the mix, to act as the main Hadoop server - a Zotac Zbox PC with 4GB of memory.)

### **Static IPs**

On all of the PCs, I added a static IP, using the DHCP assigned address. Before you start changing your connection info, I recommend opening a terminal window and entering the "ifconfig" command. This will show you the existing address for your PC, the gateway, the net mask and a lot of other info you probably won't care about.
<!-- more -->
Then in the CentOS desktop, right click on the network icon (looks like two computers - one on top of the other), and choose Edit Connections. The Network Connections window opens, and you click on your Wired Auto eth0 entry, and then the edit button. Make sure the Connect Automatically and Available to all users boxes are checked, then go to the IPv4 tab. Change the Method drop down to Manual. Below that enter your Address, Net mask and Gateway (usually the address of your router that connects to your ISP). Finally be sure to enter at least one value for DNS Server. I generally use the Gateway address here again because my router acts as a DNS cache. You may need to enter the DNS information provided by your ISP. All done? Click over to the IPv6 tab and set it to "Ignore" in the Method drop down. Now click on the Apply button. You should be prompted to enter a password for the root account after which the change are made. Switch back over to your terminal window, and try pinging a website on the Internet to see if you get a response. (Google.com or Yahoo.com work pretty well.) If you get a series of messages back that a certain number of bytes were received, you are good. Hit Ctrl-C to stop the ping, and exit the terminal.

### **Hostname setup**

Now that I had a static IP setup for each PC, I needed to add a hostname to each PC as well. During the CentOS installation you are asked for a hostname, but I like to make sure its all working now. First open a terminal window again. Enter "su -" to switch to superuser mode (enter your password when prompted). Now, using your favorite text editor, open up /etc/sysconfig/network. Change the Hostname line to your fully qualified domain name (FQDN). It should read:

#### HOSTNAME=<<computername>>.<<domain>>.<<domain extension>>

So for example:

#### HOSTNAME=bob.aaaaaa.com

Save the file and exit back to the terminal. Type the command "hostname" and you should be rewarded with the entry you just put into the /etc/sysconfig/network file. Repeat on each system, using a unique computer name for each. Generally its a good idea to use the same domain and domain extension for all of the systems on your local network, but the computer name has to be different.

### **Host file**

If you have a DNS server setup on your network, you will need to enter the various server names into your DNS cache. If however, you have a smaller network like me, you can get around this by editing the HOSTS file on your boxes. The HOSTS file is like a shortcut DNS. Its a pain to keep up to date on a large network, but for the small four node cluster I am setting up, its not a big deal. Using your favorite text editor again, add lines to the end of the HOSTS file for each box. The syntax is: IP Address    FQDN    Hostname So for example:

#### 192.168.0.1     bob.aaaaa.com bob

Once you have all of the systems entered, save the file and exit to the terminal. Try pinging one or more of the systems you just added to the Hosts file. If you get responses back, you can move on to the next one. If you don't, examine the file and make sure you have the correct IP address and host. **TIP: I advise getting a small label maker and printing all labels with the PC name and the IP address and affixing them to the front of each computer. It makes it much easier to do this setup.**