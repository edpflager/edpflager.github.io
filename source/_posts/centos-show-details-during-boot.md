---
title: Centos - Show details during boot
tags:
  - goofy
  - How-to
  - howto
  - SysAdmin
  - technical
id: '2646'
categories:
  - - Blog
  - - Linux
comments: false
date: 2015-01-09 19:02:51
---

[![startup](http://edpflager.com/wp-content/uploads/2015/01/startup-300x167.png)](http://edpflager.com/wp-content/uploads/2015/01/startup.png)By default CentOS 6 shows an animation while the system boots up, indicating its progress with either a rotating ring or a progress bar (in my experience physical machine installs show the rings, and VM installs show the progress bar). However, if you are from a sysadmin background or are responsible for monitoring one or more CentOS boxes, you may want to see what's happening while the system comes up, rather than a simple animation.
<!-- more -->
If you want to see the startup progress one time, just hit the up arrow key on your keyboard, and the animation will disappear and be replaced with scrolling text showing you what is happening on the box. If you want the text mode to display every time you start the system, the process is pretty straightforward, and only takes a couple of minutes to enable.

1.  Open a terminal and switch to Super User mode with:
    
    ```
    su -
    ```
    
    When prompted, enter your password.
2.  Type this command:
    
    ```
    plymouth-set-default-theme details <ENTER>
    ```
    
3.  Next type this command:
    
    ```
    /usr/libexec/plymouth/plymouth-update-initrd <ENTER>
    ```
    
    The system will process for a few minutes (on my VM it took about a minute), and eventually the command prompt will be returned.
4.  That's it. The next time you start the system the animation will be replaced with a scrolling screen indicating the progress of various system processes as they start. If you want to switch back to the rings/progress bar, reenter the commands, and replace "details" in the first command with "rings" .

So what did the commands above do? The first command tells the plymouth application to use the details theme as its default. Plymouth is the program that shows the animation at boot up and it interacts with  the initrd process while the system starts. For a more detailed explanation of plymouth, check out this [link](http://www.freedesktop.org/wiki/Software/Plymouth/). The second command updates the plymouth process to use the new default. There are other plymouth themes available and you can even create your own if you are so inclined.