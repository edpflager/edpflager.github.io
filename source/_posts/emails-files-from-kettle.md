---
title: Emails files from Kettle
tags:
  - ETL
  - How-to
  - howto
  - kettle
  - PDI
  - technical
id: '2357'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
  - - Pentaho
comments: false
date: 2014-08-22 19:00:11
---

[![mailenvelope](http://edpflager.com/wp-content/uploads/2014/08/mailenvelope-298x300.jpg)](http://edpflager.com/wp-content/uploads/2014/08/mailenvelope.jpg)A common task I encounter when working with ETL tools is to send output files somewhere. I often have to FTP files, but just as often, I need to email output files. I'll cover how to FTP in a future post, but this time I'll walk through how to set up a job in Pentaho Kettle (aka Pentaho Data Integration or PDI) to email data files. Unlike the "Put FTP" step in PDI, where you can specify the file or files you want to upload as part of the job component, when sending files via email, you have to create  a transformation step to define the files you want to send, and then pipe that information into the Email step. This is similar to how variables work in Pentaho, where you define the variables in a step before you can use them. If this is something you need to do, and you want to know how to do it, read on! At its most basic level, this kind of task in PDI is very simple building on the task of creating files in PDI, whether they are text , Excel, or whatever. Once the output files are created, sending them via email involved only a couple of steps.
<!-- more -->
##### DEFINE THE FILES TO SEND

[![definefiles](http://edpflager.com/wp-content/uploads/2014/08/definefiles.png)](http://edpflager.com/wp-content/uploads/2014/08/definefiles.png)

1.  Create a transformation step to define the file(s) you want to send. Drag a "Get File Names" component from the Input section of the Design panel in Spoon on to your work space, and then a "Set files in result" from the Job section. Create a hop between the two components.
2.  Double click the Get File Names component to configure it. On the Files tab of the window that opens, click on the BROWSE button on the lower right side. Navigate to where the files are located that you want to send. You can also enter a UNC path if the files are on another computer in the format of: \\computernamesharefoldersubfolderfilename.
3.  Click on the first file, and click Open to return to the "Get File Names" window. If you are only sending one file, and the name is not likely to change, go ahead and click the ADD button on the lower right side. The file and path will be moved down to the Selected Files grid. If you have other files you want to send repeat this process. ![GetFileNamesWildcard](http://edpflager.com/wp-content/uploads/2014/08/GetFileNamesWildcard.png)
4.  If you are adding a number of files that follow a common naming convention, or if the filename is likely to change you can Regular Expressions to allow Kettle to figure out the file names for you. As an example, if I was going to send a number of .png (graphics) files that reside in the same folder, in the Selected Files grid, I could remove the file name from the first cell, leaving only the path to  the folder where the files are. Then in the second cell, I can add the regular expression: ^.\*png to signify send all PNG files in that folder.
5.  Once you have added all of the files to the Get Files window, click the Preview Rows button to get a dialog box showing you the names of the files PDI thinks you want to send. If all looks good, then click Close on the preview window, and the OK on the Get Files window. (There are a lot of other options here that I won't be covering, but feel free to experiment!) ![SetFileNames](http://edpflager.com/wp-content/uploads/2014/08/SetFileNames.png)
6.  Open the Set Files component and a window will appear labeled Copy filesnames to result (confusing - I think that may be a bug). You can name the step whatever you like as long as its meaningful in the scope of your job. From the "Filename field" drop down list, select filename. You'll see there are other options you can use here to define the file(s) but for our purposes, we are defining the particular file name(s) to use. In the "Type of file to" list, select GENERAL. Click OK to return to your work space.
7.  Save your transformation.

#####  CREATE THE TRANSMIT JOB

1.  After the transformation is setup that defines the files to transmit, create a new job (or add the following to an existing job) and drag a transformation step on to your workspace from the design panel. When creating a new job, be sure to add a START step first, and define a hop from that to the transformation. [![transforminfo](http://edpflager.com/wp-content/uploads/2014/08/transforminfo.png)](http://edpflager.com/wp-content/uploads/2014/08/transforminfo.png)
2.  Open the transformation step and define the parameters to use the transformation created above. (If a repository is not used to store  transformations and jobs, the only option will be to navigate to where the KTR file was saved). Once the transformation info is entered, click OK to return to the job workspace.
3.  From the Mail node in the Design panel, drag a Mail component onto your workspace. Create a hop from the GetFIles component to the Mail component. [![mailaddresses](http://edpflager.com/wp-content/uploads/2014/08/mailaddresses-300x277.png)](http://edpflager.com/wp-content/uploads/2014/08/mailaddresses.png)
4.  Double click the Main component to open it. On the first panel, define the destination and sender email information. [![mailservertab](http://edpflager.com/wp-content/uploads/2014/08/mailservertab-300x277.png)](http://edpflager.com/wp-content/uploads/2014/08/mailservertab.png)
5.  On the Server tab, define the information for the SMTP server. Check with the email server administrator for this information. At the very least, an SMTP server name and port must be defined. [![mailmessage](http://edpflager.com/wp-content/uploads/2014/08/mailmessage-300x277.png)](http://edpflager.com/wp-content/uploads/2014/08/mailmessage.png)
6.  Click to the Mail Message tab, and enter the appropriate information. Generally its a good idea to enter a subject and a referencing message to make sure the recipient knows what the files are. [![mailattachfiles](http://edpflager.com/wp-content/uploads/2014/08/mailattachfiles-300x276.png)](http://edpflager.com/wp-content/uploads/2014/08/mailattachfiles.png)
7.  Lastly, switch to the Attached Files tab. Check the box labeled "Attach file(s) to message?". In the Select File Type pane, click on General to highlight it. This will let Pentaho know to attach the files that were defined as General in the previous step of the job. Click OK to return to the Job works space.
8.  Drag a "Success" job step on to your workspace, and add a hop from the "Mail" component to it. Save the job. [![emailfilesjob](http://edpflager.com/wp-content/uploads/2014/08/emailfilesjob.png)](http://edpflager.com/wp-content/uploads/2014/08/emailfilesjob.png)
9.  If everything has been setup correctly the job will look like the image above and when run, the job will email the files that were defined in the Get Files step to the intended recipients.

Note: Pentaho and Pentaho Data Integration are trademarks of Pentaho Inc.