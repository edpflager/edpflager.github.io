---
title: Linux Partitioning
tags:
  - guides
  - How-to
  - install
  - Mint
  - SysAdmin
  - technical
  - Ubuntu
id: '2960'
categories:
  - - Blog
  - - Linux
comments: false
date: 2015-09-15 18:14:14
---

[![shoji-screens-1416865](http://edpflager.com/wp-content/uploads/2015/09/shoji-screens-1416865-1024x768.jpg)](http://edpflager.com/wp-content/uploads/2015/09/shoji-screens-1416865.jpg)When installing a Linux distro, one of the things you have to decide on is how to partition your hard drive to store various components of the Linux system. For those new to Linux, you can let the installer decide for you, and as with most default settings the outcome may not be the best but it will work. The system's default layout generally will define a boot partition and a swap location, and then a root partition for everything else. Not optimal, but it will work. Once you've worked with Linux for a while, and have installed a few distros or upgraded, you realize that those default partitions can cause some problems. Specifically your personal files from your home folder will get overwritten and you may lose any personalized configuration settings that are stored in home hidden folders and files. But defining a partition scheme can be a daunting task. So here are some suggestions on how to partition your drive, using my current setup as an example.
<!-- more -->
For more recent PCs, you will probably need an EFI partition if the computer is designed to comply with the Unified Extensible Firmware Interface. This partition stores files that are used to start installed operating systems or other utilities. To allow for multiple operating systems or other additions, set aside 20GB for the EFI partition. Boot partition - I have seen a number of recommendations for sizing the boot partition, but generally they fall in the 300MB to 500MB range. Kernel images and other files that are actually used to boot the operating system are stored here. If you plan to dual or multiboot on your system, go on the high side of that recommendation. Root or system partition - the files from the boot partition access the system partition. Here is where the actual operating system files are stored. For most current Linux distros, a system partition of 15-30GB should be sufficient. Its also possible to have the Boot and System partitions as one partition if you want. If you plan to install a large amount of software on your machine, you may want to make this partition larger. Home partition - as I stated above, the Home partition is where your files and configuration settings are stored. Generally you want this to  be the largest partition, since you will have your files here. On my system, this partition is about 180GB because I usually work with several virtual machines which are stored in my home folder. SWAP space - a good rule of thumb for sizing your swap space partition is to take the amount of RAM in  your system and double it. So for my system, with 8GB of RAM, I setup a 16GB swap partition.  Summary:

*   /EFI Boot       20GB
*   /Boot            300MB
*   /  (Root)          20GB
*   /Home          180GB
*   /Swap             16GB

Total ~                             236GB That's it!