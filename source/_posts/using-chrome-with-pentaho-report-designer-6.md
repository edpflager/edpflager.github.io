---
title: Using Chrome with Pentaho Report Designer 6
tags:
  - guides
  - How-to
  - Linux
  - technical
id: '3012'
categories:
  - - Blog
  - - Business Intelligence
  - - Pentaho
comments: false
date: 2015-11-07 16:28:24
---

[![report](http://edpflager.com/wp-content/uploads/2015/11/report-300x251.jpg)](http://edpflager.com/wp-content/uploads/2015/11/report.jpg)Pentaho's Report Designer (PRD) is a full featured application that allows you to define reports that can be used within the Pentaho BI suite or as stand-alone documents. Output can be in a number of formats: PDF, Excel (XLS or XLSX versions), CSV/TXT, RTF or HTML.  If you would like to do a preview of your report in HTML format and you don't have one of the default supported browsers installed (like me on my Mint laptop), or you would like to use a different default browser, you can tell PRD which browser to use. Open Report Designer, and from the main menu, click on EDIT, and then click the Preferences option at the bottom of the screen.
<!-- more -->
The SETTINGS window will open (not Preferences). On the left side, click the BROWSER button. The Settings window will update and show you browser options, as below. Switch the option from Default Browser to User defined Browser. In the program window you can enter the path to your browser. If you are not sure where exactly the browser is on your system, you can click the Browse button (looks like three dots) and navigate to the folder. On my system Chrome is installed under /usr/bin. Leave the parameters field as it is. On my system the results are pictured below. [![PRD-Browser](http://edpflager.com/wp-content/uploads/2015/11/PRD-Browser-300x227.png)](http://edpflager.com/wp-content/uploads/2015/11/PRD-Browser.png) Click the APPLY button at the bottom right and the window will close. You can test it out by going to the Help menu and choosing the Documentation option. If everything is OK, the browser you specified will open and the Pentaho Help page will be displayed.