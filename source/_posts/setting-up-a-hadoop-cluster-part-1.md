---
title: Set up a Hadoop Cluster - Part 1
tags:
  - CDH
  - Hadoop
  - HortonWorks
id: '1714'
categories:
  - - Big Data
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2014-01-22 15:10:05
---

[![cluster1](http://edpflager.com/wp-content/uploads/2014/01/cluster1.png)](http://edpflager.com/wp-content/uploads/2014/01/cluster1.png)I have posted a few times about various aspects of my experimenting with Hadoop and other Big Data platforms. Between Thanksgiving and Christmas, I even maxed out the memory of my desktop PC (a Mac Mini) so I could run multiple virtual machines to act as a Hadoop cluster. It worked, but performance was poor. Since then, I've been looking into getting a small number of cheap PCs to setup as a physical cluster.
<!-- more -->
About two weeks ago, I found a [good deal](http://www.newegg.com/Product/Product.aspx?Item=N82E16883250715) on refurbished HP desktops with a 2.0 GHz dual core 64 bit processor.  The memory wasn't stellar at 2 GB each (up-gradable to  8 GB, if necessary), and an 80 GB hard drive (again up-gradable if necessary) but for a small cluster it should be doable. Preloaded with Windows 7 Home Premium 64,  I planned on using CentOS 6.5 as my operating system. (With Ubuntu as a fallback). They showed up last week, and in my free time I've been working to get the initial configuration loaded. I have had a few issues but nothing earth shattering:

*   Only one wanted to boot from CD/DVD. It may have been a BIOS setting, but I got around that by [making a bootable USB drive](http://unix.stackexchange.com/questions/81858/run-centos-6-from-a-usb-flash-drive) from the CentOS LIVE DVD.
*   Using the live DVD installs both KDE and GNOME desktop as well as a lot of other apps that you aren't likely to need. You can remove them pretty quickly using the system tools and it isn't difficult.
*   I found removing Evolution and Postfix caused the system to hand at boot-up, so I've left those installed, just disabled. Something in one of those packages seems to cause issues.

Yesterday, I finished the OS install and got the boxes hooked up to a [cheap 4 port KVM](http://www.amazon.com/gp/product/B001S2PJO6/ref=oh_details_o01_s00_i00?ie=UTF8&psc=1), a wireless keyboard (Target was clearing them out for $6 - cash) and mouse. I picked up a small wire shelving unit from Home Depot for less than $20, and using a bunch of cable ties, got the cabling under control. For a monitor, I had a small TV that also have a VGA input so no need to buy a monitor. Since most work with Hadoop is done via a web interface, I can access the system from my regular desktop once I'm finished. So that's where I'm at. Next steps are to get the software prerequisites in place, and then start installing Hadoop. Stay tuned for more details!