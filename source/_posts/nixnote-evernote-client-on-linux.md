---
title: NixNote (Evernote client) on Linux
tags:
  - howto
  - install
id: '2072'
categories:
  - - Blog
  - - Linux
comments: false
date: 2014-05-12 03:57:10
---

[![Nixnote](http://edpflager.com/wp-content/uploads/2014/05/Nixnote-300x115.png)](http://edpflager.com/wp-content/uploads/2014/05/Nixnote.png)Evernote is a cloud based service that allows you to take and share notes across all of your devices - Android, iPhones and iPads, Windows and Windows phones, and Mac OSX. Its very handy, allowing you to jot down notes when you think of them and pull them up later when you need them. I often use it to make notes for this blog on my phone and then pull them up later on my laptop when I want to work on posts. But did you notice the glaring omission in that list of products? Linux. If you are running Ubuntu, you can install [Everpad](https://github.com/nvbn/everpad) to provide a client to access your Evernote account. Everpad's wiki provides some instructions on getting it to run on other distros. You can access your account via the Everpad website as well, but I'd still rather have a dedicated client. I keep a lot of links and instructions for installations in my Everpad account, and copying and pasting is much handier than typing everything. Recently I found [NixNote](http://nevernote.sourceforge.net/) (aka NeverNote), an open source client for accessing Evernote. I experimented with it, and got it running in CentOS with only a couple of dependent packages. Below are the instructions for running NixNote on CentOS 6.5.
<!-- more -->
1.  Open the Add/Remove Software application and search for "TermReadKey". Install the one package that should result - perl-TermReadKey-2.30-13.el6 (x86-64) currently.
2.  In Add/Remove Software search for openssl. In the results, locate these two packages and install them, if they aren't already installed: "General purpose cryptography library with TLS implementation (openssl-1.0.1e-16.el6\_5.7 (x86\_64)" "Files for development of applications which use OpenSSL (openssl-devel-1.0.1e-16.el6\_5.7 (x86\_64)" - this will require 6 additional packages to be installed as well.
3.  Download the nixnote rpm package from the [NixNote sourceforge site](http://sourceforge.net/projects/nevernote/files/). Version 1.6 is the most current stable version.
4.  Open a terminal window, and switch to the root user account (su -).
5.  Navigate to where you downloaded the RPM package and run this command: **rpm -ivh nixnote-1.6-2.x86\_64.rpm**
6.  It will install very quickly, and in your menu bar, under Applications | Internet you should see an option for NixNote.
7.  Start NixNote and in the menu bar click on Tools -> Connect. A window will appear asking you to grant access to Evernote for NixNote.
8.  If you already have an Evernote account, sign in. If not click the option to create a new account.
9.  You now have access to your Evernote account from your Linux machine!