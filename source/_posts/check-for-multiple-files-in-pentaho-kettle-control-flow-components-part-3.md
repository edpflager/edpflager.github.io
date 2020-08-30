---
title: Check for multiple files in Pentaho Kettle - Control Flow components - Part 3
tags:
  - ETL
  - How-to
  - howto
  - kettle
  - PDI
  - technical
id: '2749'
categories:
  - - Blog
  - - Pentaho
comments: false
date: 2015-05-09 18:10:57
---

[![arrows](http://edpflager.com/wp-content/uploads/2015/05/arrows.jpg)](http://edpflager.com/wp-content/uploads/2015/05/arrows.jpg)A couple of posts back, I covered how to use the control flow component - "File Exists" to look for a file, and if its not found, to take appropriate actions. While useful, that component does have the limitation that it only looks for one file. If you need to check for multiple files on the local machine or somewhere on your network, you can use the "Check if files exist" component instead. This comes in handy if your workflow requires more than one file, or if different actions can be taken if a file is missing or not.  Below I'll walk through creating a simple workflow to illustrate the usage of this step.
<!-- more -->
1.  Start Spoon and create a new job.
2.  From the General node in the component panel (the left panel in Spoon), drag a "Start" and a "Success" component on to the canvas.
3.  Open the Utility node, and drag an "Abort job" component to the canvas.
4.  Finally, add a "Check if files Exist" component from the Conditions node.
5.  Add an unconditional hop between the "Start" component to "Check if files Exist".
6.  From "Check if files Exist", add a hop to the "Success" component. Right click the hop, and a menu will appear. Click the Evaluation option to set  "Follow when result it true."
7.  At this point, your workflow should look similar to this:[![CheckIfExist](http://edpflager.com/wp-content/uploads/2015/05/CheckIfExist-300x127.png)](http://edpflager.com/wp-content/uploads/2015/05/CheckIfExist.png)
8.  Double click the "Check if files exist" component to configure it. You be presented with this window:[![CheckFilesExist2](http://edpflager.com/wp-content/uploads/2015/05/CheckFilesExist2-300x117.png)](http://edpflager.com/wp-content/uploads/2015/05/CheckFilesExist2.png)
9.  Click the FILE button to navigate to where the file will be located. If the file is on a different computer than the one Kettle (PDI) will be running on, and you are running Linux or Mac OS X, you'll need to mount the file location as a Volume. If you are using Windows, you can map a drive, or use an UNC path to the file.  (NOTE -  I've tried using file:\\\\,  cifs:\\,  smb:\\ , afb:\\ and vfs:\\ along with a number of other permutations with no success trying to access files over a network under Mac OS X and Linux, and the only way I could get it to work was using a mounted location as a volume, so it appears to be part of the local filesystem).
10.  Once you have a location entered in the File/Folder name box, click the ADD button to move it to the grid at the bottom of the window.
11.  Repeat steps 9 and 10 until you have all of the files listed that you want to look for in your workflow. At that point, click OK to close the window and return to the canvas.
12.  Save your work and run the job to test it. If all the files you entered are found, the job will take the path to the Dummy step. If even one of the files are not found, the Abort Job step will be called.