---
title: Setup a Single-node Hadoop Yarn machine using CDH5 - Part 1
tags:
  - CDH
  - Cloudera
  - install
  - SysAdmin
  - technical
  - YARN
id: '2475'
categories:
  - - Big Data
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2014-10-05 15:44:11
---

[![Balls of yarn](http://edpflager.com/wp-content/uploads/2014/10/yarn-300x195.jpg)](http://edpflager.com/wp-content/uploads/2014/10/yarn.jpg)Previously I posted a series of articles that walked through installing a single-node Hadoop machine using [version 1 of MapReduce](http://edpflager.com/?p=1945 "Setup a Single-node Hadoop machine using CDH5 and HUE – Part 1"). Since that  series went live, a good deal of development has moved on to MRv2 aka YARN. I won't go into the intricacies of the new architecture, suffice to say that it aims to be more efficient than the older MRv1. This article and any following it are based on the web documentation from Cloudera. While their information is very thorough, it often jumps around making it difficult for someone new to Hadoop to follow. Hopefully the instructions I will be providing here will make it easier for others to setup as well. Please be careful if you are copying lines from these articles to paste into your Hadoop config files. I have found that the double hyphen characters used in the comment lines may copy over as a long hyphen instead. This is likely to cause issues when attempting to run the various components. Some may ask, "What's the point?" Cloudera, Hortonworks and other vendors offer Sandbox VM's of their distribution that you can download and play with so why install it yourself? My answer to that is, by installing it myself, I can learn more about how it all fits together, and gives me a better understanding of the whole Hadoop ecosystem. WARNING: This is not meant to be a production system, and I provide no warranties as to the usability of the system once you are complete. If you are setting up a production system and/or one that will exposed outside your firewall, please make sure you enable adequate security measures!
<!-- more -->
### **Prepare your single node for Hadoop**

1.  Install a CentOS 6.5 64-bit server with the standard Gnome desktop. A minimum of 2GB of RAM is necessary, although 4GB or more is better. Hard drive space should be a minimum of 20GB, considerably more if you are planning on experimenting with any sizable data sets.
2.  Remove any unneeded applications and install any system updates.
3.  Enable NTP (network time protocol daemon) via the Service Configuration window so your system time stays in sync with an external time source.
4.  Disable iptables and iptablesv6 from the Service Configuration window.
5.  Edit [/etc/sysconfig/selinux](http://edpflager.com/?p=1866 "Disable SELinux to install Cloudera") and set it to disabled. Restart your server for all the config changes to take effect.
6.  Remove the preinstalled Java runtime engines from the Add/Remove Applications tool by searching for "OpenJDK". Once you have completed that, open a terminal window and type: **          java -version** You should receive a message that nothing was found.
7.  Download the Java Developers Kit 7u45 Linux-x64 RPM from this link (you may have to create a free Oracle account to download):   [http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html)
8.  Install the java sdk file by opening a terminal window and switching to the root user account: **          su -** Navigate to where the RPM file was downloaded to and then enter this command to install JAVA: **          rpm -ivh ./jdk-7u45-linux-x64.rpm**
9.  Once the installation completes repeat step 6, and it should report that java version 1.7.0\_45 is installed.
10.  You now need to add a static IP address to your system. If your network administrator has provided one for you, use it. Otherwise to get the existing address for your PC, the gateway, the net mask and a lot of other info in the  terminal window, enter the command: **           ifconfig**
11.  On the CentOS desktop, right click on the network icon in the top panel and choose Edit Connections. When the Network Connections window opens, click on the eth0 entry, and then the edit button.
12.  Make sure the Connect Automatically and Available to All Users boxes are checked, then go to the IPv4 tab. Change the Method drop down to Manual. Below that enter the Address, Net mask and Gateway that ifconfig displayed.
13.  Be sure to enter at least one value for DNS Server. I generally use the Gateway address here again because my router acts as a DNS cache. You may need to enter the DNS information provided by your ISP.
14.  Click over to the IPv6 tab and set it to “Ignore” in the Method drop down. Now click on the Apply button. You should be prompted to enter a password for the root account after which the change are made.
15.  Switch back over to your terminal window, and try pinging a website on the Internet to see if you get a response. If you get a series of messages back that a certain number of bytes were received, you are good. Hit Ctrl-C to stop the ping.
16.  While you are still logged in as root, edit the hosts file (/etc/hosts) to set your host name in the format: **        ipaddress     fully qualified domain name       short name** As an example: **        10.0.1.10     cloudera.mydomain.com           cloudera**
17.  Edit the **/etc/sysconfig/network** file to make sure your hostname is also set there. If you supplied a host name during the OS installation, it should already be defined.
18.  Still as root in the terminal window, edit the root .profile file (~/.bash\_profile) and add these two lines to the end of the file, after "export PATH" line: **         export JAVA\_HOME=/usr/java/latest** **         export PATH=$JAVA\_HOME/bin:$PATH**
19.  After you save the file, exit the root user account by typing Exit in the terminal.
20.  With the terminal still open, re-enable root access to force the bash profile to be reloaded and check that the java\_home value is set correctly. Type: **        echo $JAVA\_HOME**
21.  The system should respond with: **/usr/java/latest**

Stop and take a breath.  At this point you have prepared your system for Hadoop. If you are working in a virtual machine, now would be a good time to make a backup or snapshot.

### **Install CDH5**

1.  Open a web browser and download the CDH 5 package for Red Hat from this link: [http://archive.cloudera.com/cdh5/one-click-install/redhat/6/x86\_64/cloudera-cdh-5-0.x86\_64.rpm](http://archive.cloudera.com/cdh5/one-click-install/redhat/6/x86_64/cloudera-cdh-5-0.x86_64.rpm)
2.  In the terminal window navigate to where the RPM file was downloaded. and enter this command (that's two dashes after YUM): **        yum --nogpgcheck localinstall cloudera-cdh-5-0.x86\_64.rpm** The CDH5 quick start package will be installed.
3.  Now add the Cloudera repository GPG key (that's two dashes after RPM): **       rpm --import  [http://archive.cloudera.com/cdh5/redhat/6/x86\_64/cdh/RPM-GPG-KEY-cloudera](http://archive.cloudera.com/cdh5/redhat/6/x86_64/cdh/RPM-GPG-KEY-cloudera)**
4.  And finally install Hadoop with MRv1 with this command: **       sudo yum install hadoop-conf-pseudo** (At present 18 packages will be downloaded and installed so the time involved may take a few minutes depending on your Internet connection.)
5.  Verify that Hadoop is installed correctly. At the command prompt enter **rpm -ql hadoop-conf-pseudo** and you should see a list of hadoop folders. No errors? All good.
6.  Format the HDFS filesystem with this command to make sure Hadoop is working correctly: **       sudo -u hdfs hdfs namenode -format** If you receive any error messages, go back through the instructions above to make sure you have down them correctly.
7.  With the HDFS file system formatted, you can start the various Hadoop services, by entering this command at the terminal prompt: **for x in \`cd /etc/init.d ; ls hadoop-hdfs-\*\` ; do sudo service $x start ; done** (Watch the angle of the quotes - I have found that using normal single quotes errors out).
8.  Open a web browser and point it to your your system name and port 50070. For example, if your system was called HadoopTest, you would enter http://HadoopTest:50070 in your browser. In CDH version 5, you'll see a website with multiple tabs at the top that you can click through to check out the status of your Hadoop box. (See the graphic at the top of this article).
9.  Now you need to create tmp, staging and log folders in HDFS and set permissions. With the terminal window open, enter: **   sudo -u hdfs hadoop fs -mkdir -p /tmp/hadoop-yarn/staging/history/done\_intermediate** **   sudo -u hdfs hadoop fs -chown -R mapred:mapred /tmp/hadoop-yarn/staging** **   sudo -u hdfs hadoop fs -chmod -R 1777 /tmp** **   sudo -u hdfs hadoop fs -mkdir -p /var/log/hadoop-yarn** **   sudo -u hdfs hadoop fs -chown yarn:mapred /var/log/hadoop-yarn**Be sure to use the sudo -u hdfs portion of the commands to ensure that the proper user creates the folders.
10.  Once that is complete, you can start MapReduce with these commands: **   sudo service hadoop-yarn-resourcemanager start** **   sudo service hadoop-yarn-nodemanager start** **   sudo service hadoop-mapreduce-historyserver start**
11.  Create the /user home directory, and then create a home directory for  yourself in the HDFS file system, and change the owner to yourself as well: **   sudo -u hdfs hadoop fs -mkdir /user** **   sudo -u hdfs hadoop fs -mkdir /user/<username>**
12.  And finally change the ownership of the user's home directory, to the specific user: **sudo -u hdfs hadoop fs -chown <your user name> /user/<your user name>**
13.  Reopen a web browser and point it to your system name and port 50070. For example, if your system was called HadoopTest, you would enter http://HadoopTest:50070 in your browser. In CDH version 5, you'll see a website with multiple tabs at the top.
14.  Click on the Datanodes tab and you should see your local machine listed along with the disk space available.
15.  Click on the Utilities tab, and select the Browse the file system option, and you can see the various folders that were created in the steps above. Click on the User folder link, and you should see the various user folders and who the owners are.

If you've reached this point, you should now have a running Hadoop Yarn installation in pseudo-cluster mode. Next time, we'll start the installation of the various GUI tools to allow you to interact with your system via a web browser.