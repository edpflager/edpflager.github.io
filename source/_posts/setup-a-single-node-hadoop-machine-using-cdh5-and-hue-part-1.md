---
title: Setup a Single-node Hadoop machine using CDH5 and HUE - Part 1
tags:
  - CDH
  - Cloudera
  - Hadoop
  - HUE
id: '1945'
categories:
  - - Big Data
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2014-04-14 15:15:31
---

[![Hadoop](http://edpflager.com/wp-content/uploads/2014/04/Hadoop-300x191.png)](http://edpflager.com/wp-content/uploads/2014/04/Hadoop.png) I've been awaiting the open source introduction of Cloudera's Hadoop distribution, CDH 5,  to try installing a pseudo-distributed cluster using CDH, with the HUE GUI interface. (If you are not familiar with the terminology, pseudo-distributed mode allows you to run Hadoop on one machine, with the various daemons each running in a separate JVM.) By setting up a pseudo-distributed cluster, I could free up two other machines for other projects I'm working on. Update: Please be careful if you are copying lines from these articles to paste into your Hadoop config files. I have found that the double hyphen characters used in the comment lines may copy over as a long hyphen instead. This is likely to cause issues when attempting to run the various components.
<!-- more -->
I've managed to install a vanilla version of Hadoop with Hive previously, but I couldn't get Cloudera's distribution to work with the extra bells and whistles of HUE, HBase and Impala. With the introduction of a new version, it seems like a good time to try it. The good news is, I have it working successfully, on two different machines! Yeh! The bad news, its not a simple process, but it is doable if you have a few hours to spare. The biggest drawback for me was trying to navigate Cloudera's website to get the information I needed to make this work. (Having worked in IT for more than a dozen years, I'm used to challenging documentation.) Hopefully the instructions I will be providing here will make it easier for others to setup as well. This series is based on the Cloudera documentation, but I've modified it to be easier to follow for setting up a pseudo cluster. Some may ask, "What's the point?" Cloudera offers a Sandbox VM of their distribution that you can download and play with (although at this writing it has not been updated to CDH5) so why install it yourself. My answer to that is, by installing it myself, I can learn more about how it all fits together, and gives me a better understanding of the whole Hadoop ecosystem. WARNING: This is not meant to be a production system, and I am providing no warranties as to the usability of the system once you are complete. If you are setting up a production system and/or one that will exposed outside your firewall, please make sure you enable adequate security measures! With that said, here is part one.

### **Prepare your single node for Hadoop**

1.  Install a CentOS 64-bit server with the standard Gnome desktop. A minimum of 2GB of RAM is necessary, although 4GB or more is better. Hard drive space should be a minimum of 20GB, considerably more if you are planning on experimenting with any sizable data sets.
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

Stop and take a breath. Get a drink. At this point you have prepared your system for Hadoop. If you are working in a virtual machine, now would be a good time to make a backup or snapshot.

### **Install CDH5**

1.  Open a web browser and download the CDH 5 package for Red Hat from this link: [http://archive.cloudera.com/cdh5/one-click-install/redhat/6/x86\_64/cloudera-cdh-5-0.x86\_64.rpm](http://archive.cloudera.com/cdh5/one-click-install/redhat/6/x86_64/cloudera-cdh-5-0.x86_64.rpm)
2.  In the terminal window navigate to where the RPM file was downloaded. and enter this command (that's two dashes after YUM): **        yum --nogpgcheck localinstall cloudera-cdh-5-0.x86\_64.rpm** The CDH5 quick start package will be installed.
3.  Now add the Cloudera repository GPG key (that's two dashes after RPM): **       rpm --import  [http://archive.cloudera.com/cdh5/redhat/6/x86\_64/cdh/RPM-GPG-KEY-cloudera](http://archive.cloudera.com/cdh5/redhat/6/x86_64/cdh/RPM-GPG-KEY-cloudera)**
4.  And finally install Hadoop with MRv1 with this command: **       yum install hadoop-0.20-conf-pseudo** (At present 16 packages will be downloaded and installed so the time involved may take a few minutes depending on your Internet connection.)
5.  Verify that Hadoop is installed correctly. At the command prompt enter                       **rpm -ql hadoop-0.20-conf-pseudo** and you should see a list of hadoop folders. No errors? All good.
6.  Format the HDFS filesystem with this command to make sure Hadoop is working correctly: **       sudo -u hdfs hdfs namenode -format** If you receive any error messages, go back through the instructions above to make sure you have down them correctly.
7.  With the HDFS file system formatted, you can start the various Hadoop services, by entering this command at the terminal prompt: **     for x in \`cd /etc/init.d ; ls hadoop-hdfs-\*\` ; do sudo service $x start ; done**  (Watch the angle of the quotes - I have found that using normal single quotes errors out).
8.  Open a web browser and point it to your your system name and port 50070. For example, if your system was called HadoopTest, you would enter http://HadoopTest:50070 in your browser. In CDH version 5, you'll see a website with multiple tabs at the top that you can click through to check out the status of your Hadoop box. (See the graphic at the top of this article).
9.  Now you need to create a temp folder in HDFS and set its permissions. This is extremely important, as a number of components really on this folder to work properly. With the terminal window open, enter: **sudo -u hdfs hadoop fs -mkdir -p /tmp** **sudo -u hdfs hadoop fs -chmod -R 1777 /tmp** Be sure to use the sudo -u hdfs portion of the commands to ensure that the proper user creates the folders.
10.  Next, before you start MapReduce, you need to create its system directories. Enter these three commands one after another in the terminal: ** **sudo -u hdfs hadoop fs -mkdir -p /var/lib/hadoop-hdfs/cache/mapred/mapred/staging**** ****sudo -u hdfs hadoop fs -chmod 1777 /var/lib/hadoop-hdfs/cache/mapred/mapred/staging****        **sudo -u hdfs hadoop fs -chown -R mapred /var/lib/hadoop-hdfs/cache/mapred**
11.  Once that is complete, you can start MapReduce with this command (again be careful of the quotes): **    for x in \`cd /etc/init.d ; ls hadoop-0.20-mapreduce-\*\` ; do sudo service $x start ; done** If all goes well, you'll see messages indicating Hadoop jobtracker and Hadoop tasktracker have started OK.
12.  Open a web browser and point it to your your system name and port 50030. So for example, if your system is called HadoopTest,  you would enter http://HadoopTest:50030 in your browser. You should see a website that shows the condition of your MapReduce system.
13.  Finally, create a home directory for yourself in the HDFS file system, and change the owner to yourself as well: **sudo -u hdfs hadoop fs -mkdir -p /user/<your user name>** **sudo -u hdfs hadoop fs -chown <your user name> /user/<your user name>** 

If you've reached this point, you should now have a running Hadoop installation in pseudo-cluster mode. From a command line, you can access the Hadoop interface, and submit mapreduce jobs. (Limited by your RAM and hard drive space). Next time, we'll start the installation of the various GUI tools to allow you to interact with your system via a web browser.