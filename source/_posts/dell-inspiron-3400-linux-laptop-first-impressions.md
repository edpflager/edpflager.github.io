---
title: Dell Inspiron 3400 Linux laptop - First Impressions
tags:
  - SysAdmin
  - Ubuntu
id: '2883'
categories:
  - - Blog
  - - Linux
comments: false
date: 2015-07-18 19:05:28
---

## [![Dell Inspiron 14 3451](http://edpflager.com/wp-content/uploads/2015/07/Dell-Inspiron-14-3451-300x162.jpg)](http://edpflager.com/wp-content/uploads/2015/07/Dell-Inspiron-14-3451.jpg)Dell Inspiron 3400 Linux laptop - First Impressions

For years, many of us who wanted to run Linux on reasonably priced hardware had to buy systems with Windows already installed on it, and then wipe the system to put our chosen distro on it. While some manufacturers have offered systems with Linux, they tend to be higher end machines, with a matching higher end price tag. You also had the option to build your own system, but that was time consuming for a lot of us. Earlier this year, Dell announced two laptop lines with a lower price tag, the Inspiron 14 inch 3400 Ubuntu Edition and Inspiron 15 inch 3500 Ubuntu Edition series. All of them come with a slightly modified Ubuntu 14.04 LTS operating system and with options for Intel Celeron or Pentium processors, 2 or 4 GB of RAM and a 500 GB hard drive. All of the systems are very affordable at $225-$350 dollars. **UPDATE: After some research, I found that the Pentium processor in the 14 inch model does support up to 8GB of RAM. I swapped out the 4GB stick with an 8GB one I had from a ZBOX machine, and I am happy to report that it functions fine with 8GB! ** With that information in hand, I ordered a 14 inch system with the Intel Pentium  N3540 processor and 4 GB of RAM and received it just before the Independence Day holiday. I've spent the last couple of weeks poking and prodding it, and putting it through its paces.
<!-- more -->
Its been noted online that the modified Ubuntu installation has some issues, and I experienced many of the same problems. The biggest issue is with the steps that are supposed to allow you to create a restore diskette. Eventually I gave up on that, and decided to swap out the original hard drive with another one so I would have the original one if necessary to fall back on. I dropped a 120 GB SSD drive in place of stock 500 GB drive and went to work trying out different live DVDs CentOS 7 worked fine, detecting the hardware with no issues. Although I'm a fan of CentOS 6, the newer version has some usability changes that I don't care for. Other live ISOs worked OK as well, except for the touchpad. Mint 17, Fedora 22 workstation and Ubuntu 15.04 all failed to recognize the touchpad. While not a huge issue for me because I prefer to use a mouse, it still could be seen as a very serious issue for others. I tried a 14.04 Ubuntu Live disk and it also had the same issue. Strange! Eventually I found a [post by Australian blogger Cain Hill](http://www.cainhill.com.au/blog/fixing-my-dell-inspiron-3451-touchpad-on-ubuntu-14.10/) that pointed to a fix for this issue. I have since verified that it does fix the issue using multiple Linux installations, not just Ubuntu.

1.  Open your system's blacklist file by running the following command in Ubuntu's Terminal application: **sudo gedit /etc/modprobe.d/blacklist.conf**
2.  Add the following line to the file: **blacklist i2c-hid**
3.  Save and restart, the touchpad will begin working instantly.

After some research, from what I can piece together, the touchpad has two different control interfaces. Because these Linux distros detect both interfaces, the two drivers conflict and neither one works correctly. By adding the line to the blacklist.conf file, you are effectively telling the OS not to use one of the drivers. Since getting that resolved, I've installed Ubuntu 15.04 with a number of different applications (Gimp, Chromium instead of FireFox, MySQL server and Workbench, SQuirreL SQL client, and Pentaho Data Integration to name a few).

##### Impressions?

*   I wish Dell offered an SSD drive instead of the 500 GB drive that it came with, but the slower drive does allow Dell to keep prices down. But with the SSD, the time it takes from pressing the power button to when the Ubuntu login screen appears is less than 14 seconds.
*   Starting at 3.9 pounds, the 14 inch model is fairly lightweight (although still heavier than the XPS 13 inch Ubuntu developer laptop that weighs in around 2.6 pounds). By replacing the stock hard drive with an SSD drive reduces the weight a little bit.
*   The max screen resolution of 1366 x 768 resolution is adequate and the Truelife LED-Backlit display looks good, for development or watching a video. Overall the display is not great, but not bad.
*   The keyboard is full size with good spacing, and responsiveness. My only quibble is that the keyboard includes a Windows key. Couldn't they put a Tux picture on there instead of that? :)
*   Sporting a 40 WHr, 4-Cell Battery Battery, the system says it has  6.5 hours on a full charge, but a lot of that depends on what you are running. Expect 3-4 hours when running graphics or processor intensive applications.
*   Some miscellaneous specs are: the system is less than an inch high when closed, making it a very low form factor. It has 2 USB 2.0 ports on the right side, 1 USB 3.0 port on the left, an HDMI out, a head phone jack , and Card reader slot. Finally it has a HD 720p capable webcam and a microphone for web conferences and video recordings.

All in all, the system is a great value for the cost conscious user looking to purchase a Linux based laptop. I recommend it.