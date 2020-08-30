---
title: Check for a file in Pentaho Kettle - Control Flow components - Part 1
tags:
  - ETL
  - How-to
  - howto
  - kettle
  - Linux
  - PDI
  - technical
id: '2716'
categories:
  - - Blog
  - - Business Intelligence
  - - Pentaho
comments: false
date: 2015-04-27 17:54:43
---

[![fileexists](http://edpflager.com/wp-content/uploads/2015/04/fileexists-300x166.png)](http://edpflager.com/wp-content/uploads/2015/04/fileexists.png)While working on a recent ETL project at my day job, I needed to include a step to check for a file before processing could begin. Pretty simple concept, but one which the tool I are using doesn't have. You have to write an external code snippet (in one of two languages), and then call that code from within the job. It just re-enforced for me why I like Pentaho Data Integration. Below I'll demonstrate how you can look for a file in Kettle, and quit the job if the file isn't found, easily with no external code. In Kettle, both the transformation palette and the job palette have a FileExists component. The Job palette also includes a component to check for multiple files. In my case, I needed to check for one file for job control purposes, so this example will demonstrate that process.
<!-- more -->
1.  Start Spoon and create a new job.
2.  From the General node in the component panel (the left panel in Spoon), drag a Start and a Success component on to the canvas.
3.  Open the Utility node, and drag an Abort job component to the canvas.
4.  Finally, add a File Exists component from the Conditions node.
5.  Add an unconditional hop between the Start component to File Exists.
6.  From File Exists, add a hop to the Success component. Right click the hop, and a menu will appear. Click the Evaluation option to set  "Follow when result it true." [![TrueCondition](http://edpflager.com/wp-content/uploads/2015/04/TrueCondition-300x110.png)](http://edpflager.com/wp-content/uploads/2015/04/TrueCondition.png)
7.  Add another hop from File Exists to the Abort component. Right click the hop and from the Evaluation option, click on "Follow when result is false".[![FalseCondition](http://edpflager.com/wp-content/uploads/2015/04/FalseCondition-300x117.png)](http://edpflager.com/wp-content/uploads/2015/04/FalseCondition.png)
8.  Double click the File Exists component to open it for the incredibly simple configuration. You can supply a name for the Job entry (I suggest supplying the file name you are looking for - i.e. Check\_for\_file\_name) in the first text box.The second box is where you enter the file name you want to check for. If you want to access a file localto the machine you are working on, just enter the path to it, preceded by "file:///"  (for example "file:///C:/Users/root/file\_name.txt" on a Windows machine, or on a Mac or Linux machine enter "file:///users/username/file\_name.txt"). Checking for a file on your local network via a UNC is almost as easy, just enter file:////servername/folder\_name/filename.csv). If you'd prefer to browse for the file or aren't sure of the path to where the file will be stored, just click the Browse button to access the Open File window.[![CheckFile](http://edpflager.com/wp-content/uploads/2015/04/CheckFile-300x268.png)](http://edpflager.com/wp-content/uploads/2015/04/CheckFile.png)
9.  In this window, you can browse your local system, or choose to look for your file in the Hadoop file system(HDFS), the MapR Hadoop distribution's file system (MapRFS), or Amazon's S3 file system. You'll be prompted to enter credentials and system information for any non-local systems . Once you have the location specified, click the OK button to return to the component. Click OK to close the component.![FileExists](http://edpflager.com/wp-content/uploads/2015/04/FileExists-300x186.png)
10.  At this point the job should look like this image. Save your work and run the job. If you entered a correct path to the file, the log will indicate the file was found and the job completed successfully.
11.  Go back into the Check Files component and alter the file name to a non-existent file. Save the job and run it again. The log will indicate the file was not found, and the Abort step was performed.

In order to use this in your own workflows, you could add an email step to the file not found branch, to let whomever is responsible for monitoring the system know that the expected file was not found. Any processing dependent on a file being found can be added to the file found branch.