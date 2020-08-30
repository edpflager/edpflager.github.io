---
title: Running Pentaho Kettle 7 on Windows 10
tags:
  - cookbook
  - ETL
  - How-to
  - howto
  - install
  - kettle
  - technical
  - Windows
id: '3475'
categories:
  - - Blog
  - - Business Intelligence
  - - Pentaho
comments: false
date: 2017-01-21 14:12:38
---

Recently I switched PCs to a newer Windows 10 based laptop for some of my work, and I wanted to get Pentaho Data Integration up and running on it. I downloaded the pdi-ce-7.0.0.0.25.zip file from the Community website, and extracted the contents to a folder in my Program Files directory. I tried running the SPOON.BAT to start it up but a window flashed on screen quickly and disappeared, but nothing else happened. I opened a command prompt and executed the SPOON.BAT file, but got a message that the JAVAW.EXE file could not be found. So I needed to perform a few other things to get it working. A quick search engine query showed me that many people had the same issue, but there didn't seem to be a consensus on how to resolve it. Below is how I managed to get it running.
<!-- more -->
First be sure to download the JAVA 64 bit version. If you just go to JAVA.com and click the Free Java Download, you are likely to get the 32 bit version which you don't want. Instead go to [this page](https://www.java.com/en/download/manual.jsp) on the JAVA.com website and download the Windows Offline (64-bit) file. Install it with the instructions that are linked next to the file download.

How to tell if you have 32 bit installed:

Open Control Panel. Switch to Icon view. Look for the Java icon. If it says 32 bit you have the wrong one installed.[![](http://edpflager.com/wp-content/uploads/2017/01/java32.png)](http://edpflager.com/?attachment_id=3476#main)

Or, if you still have the downloaded JAVA installation file, the name should have X64 in it for 64-bit. If it doesn't then its the 32 bit version.

After installing the 64 bit version of JAVA, you need to add a USER VARIABLE called JAVA\_HOME with the path to the Java install folder. Right click on the Windows icon in the lower left side of your task bar, and from the menu, choose SYSTEM to open the Control Panel - System window. Click the Advanced system setting option in the left panel. [![](http://edpflager.com/wp-content/uploads/2017/01/advancedsystem-300x117.png)](http://edpflager.com/?attachment_id=3479#main)The SYSTEM PROPERTIES window will open. Click the Environment Variables button at the bottom. [![](http://edpflager.com/wp-content/uploads/2017/01/sysproperties-262x300.png)](http://edpflager.com/?attachment_id=3480#main) The Environment Variables window will open. Click the NEW button in the middle of the window to add a new User variable. The New User Variable window will appear. In the top box type JAVA\_HOME and in the bottom box, enter the path to the JAVA installation. On my system that is: C:Program FilesJavajre1.8.0\_121. Validate the path on your system. Click OK on each window to close them. [![](http://edpflager.com/wp-content/uploads/2017/01/variable-300x253.png)](http://edpflager.com/?attachment_id=3484#main) Open a command prompt and type (without the quotes) "echo %JAVA\_HOME% " and hit ENTER. The system should respond with the path you entered previously in the Environment variables window. As a test, in the command prompt window navigate to where you extracted the KETTLE files. On my system, I would enter: cd C:Program FilesPentahodata-integration. Once there I enter: spoon.bat to run the Kettle batch file. After a few seconds the Pentaho splash screen appears and a bit later the KETTLE IDE will appear! [![](http://edpflager.com/wp-content/uploads/2017/01/kettle-1024x627.png)](http://edpflager.com/?attachment_id=3486#main)