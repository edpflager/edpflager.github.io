---
title: Running Kettle (Pentaho Data Integration) on Mac OSX 10.12 Sierra
tags:
  - ETL
  - guides
  - How-to
  - howto
  - install
  - kettle
  - Mac
  - PDI
  - technical
id: '3571'
categories:
  - - Blog
  - - Business Intelligence
  - - Pentaho
comments: false
date: 2017-05-05 16:05:02
---

[![](http://edpflager.com/wp-content/uploads/2017/05/eye-roll-300x229.jpeg)](http://edpflager.com/?attachment_id=3572#main)A new version of Mac OSX and a new version of Pentaho Data Integration (aka Kettle) but the same old problem getting Kettle to run. Apple tries to keep their operating system locked down and secure, so if you download applications from the Internet that aren't from the Apple App Store, the files are quarantined. With the update to Sierra, the quarantine process has been "improved". Keep reading to see how to do it!
<!-- more -->
Open a Finder window to where you extracted the PDI zip file. Then open a terminal prompt, next to it. In the terminal window, enter the following command:

sudo xattr -dr com.apple.quarantine

From the Finder window, drag the Data Integration.app file to the terminal window. The path to the Data Integration.app will be added to the end of the command you just entered. The result should look similar to this:

sudo xattr -dr com.apple.quarantine /Applications/data-integration/Data Integration.app

That should do it! Close the terminal window and then double click the Data Integration.app file, and shortly you'll see the Kettle splash screen.   Eye roll graphic from [clipartfest.com](https://clipartfest.com/)