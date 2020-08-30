---
title: Linux bootable USB drives on Mac OS X Yosemite - Solved!
tags:
  - How-to
  - howto
  - Mac
id: '2762'
categories:
  - - Blog
  - - Linux
comments: false
date: 2015-05-14 18:52:27
---

[![boots](http://edpflager.com/wp-content/uploads/2015/05/boots.jpg)](http://edpflager.com/wp-content/uploads/2015/05/boots.jpg)In my twenty plus years of working on PCs, I have seen external media formats change from 5.25 inch floppy disks (which really were floppy) to 3.5 inch (not-so) floppy discs to CD to DVD and USB (thumb) drives. I've had PCs at one time or another that have used all of those formats, but over the past couple of years, the push has to been to move away from external media whenever possible. Thus the last two PCs I have purchased did not come with anything other than USB slots. One is my ZBox computer that I use for development, and I change operating systems on it fairly frequently. Rather than burn a DVD of an ISO every time I want to reconfigure the development box, its should be easier to make a boot-able USB. Using Windows there are a host of different GUI tools to create bootable USB drives, and most Linux distributions are similar. If you are on a Mac running OS X Yosemite, the  solution isn't quite as simple. A number of websites include directions, but generally I've found they don't work. But this week, I found a successful method, and created both Fedora and CentOS bootable USB drives (unfortunately it doesn't seem to work with Windows ISO's). Because this requires enabling the root user account on your system, be sure to exercise caution when following these steps. Read on for more information.
<!-- more -->
1.  Find a USB drive that you are OK with erasing and insert it into one of your Mac's USB slots. Make sure the drive is formatted as Fat32 with the Boot Loader option in the partition window.
2.  Open a terminal prompt (its in your Utilities folder), and run this command: **diskutil list**
3.  Look for the USB drive you inserted, and WRITE IT DOWN! In my case it was mounted as /dev/disk3 which I was able to tell by looking at the SIZE column. In the following commands, substitute your mount point for the one I am using:[![thumbdrive](http://edpflager.com/wp-content/uploads/2015/05/thumbdrive-300x63.png)](http://edpflager.com/wp-content/uploads/2015/05/thumbdrive.png)
4.  Run this command to unmount the thumb drive, but don't remove it from the USB slot: **diskutil unmountDisk /dev/disk3**
5.  The system should respond that: **Unmount of all volumes on disk3 was successful.**
6.  Stay with me now, this next bit is convoluted! From the Apple icon in top menu bar, open System Preferences and click on **Users & Groups**. A new window will appear, showing the user accounts on your system.
7.  There is likely a padlock icon at the bottom of the window, that is closed. Click on the lock and enter your administrator user ID and password and click the unlock button to open it.
8.  At the bottom of the Users pane on the left, click on the Login Options button next to the house.[![loginoptions](http://edpflager.com/wp-content/uploads/2015/05/loginoptions-300x109.png)](http://edpflager.com/wp-content/uploads/2015/05/loginoptions.png)
9.  In the new window that appears, click the JOIN button.  The Directory Utility window will appear. Click on the OPEN DIRECTORY UTILITY button. In the new window that appears, if the padlock is closed, open it like you did previously.
10.  From the menu bar, make you are accessing the Directory Utility menu, and click on Edit, and then click Enable Root User.  (This can be a very dangerous step, since by default, Root access is disabled on a Mac! Be sure to take precautions, and once your are finished creating your bootable thumb drive, disable the root user.)
11.  You will be prompted for a password for the root account and have to enter it twice. Click OK once you are done.
12.  Switch back to the Terminal prompt and enter **su -** to switch to the root user account. When prompted, enter the password your entered for the root account in step 11.
13.  Type **dd if=** then drag and drop the iso file to the terminal window to append the file name and path.
14.  To the end of the command enter **`of=/dev/disk3 bs=1m`** substituting the correct mount point you noted in step 3. You should wind up with something like: **`dd if=/Users/Desktop/distribution.iso of=/dev/rdisk3 bs=1m`**
15.  Take a deep breath, review what you entered and if everything looks good, hit ENTER.
16.  At this point,  you'll need to wait while your USB drive is erased and the disk image is written to the drive. You'll know once its complete when the command prompt returns in your terminal windows.
17.  Once the terminal prompt returns, enter EXIT to leave root access.
18.  Switch back to the Directory Utility window, and from the menu, choose Disable Root User. Click the padlock to lock it, and then Quit Directory Utility from the menu bar.
19.  Back at the Users & Groups window, lock the padlock and then Quit System Preferences  from the menu bar.
20.   Finally, switch back to the Terminal window, and enter Exit to logout of the terminal. Quit the terminal application from the menu bar.
21.  Remove the USB drive and go boot your other PC!