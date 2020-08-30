---
title: PDI Kettle - Delete File(s)
tags:
  - ETL
  - kettle
  - PDI
id: '1918'
categories:
  - - Blog
  - - Business Intelligence
  - - Pentaho
comments: false
date: 2014-04-04 16:06:44
---

[![deletefilejob](http://edpflager.com/wp-content/uploads/2014/04/deletefilejob-300x76.png)](http://edpflager.com/wp-content/uploads/2014/04/deletefilejob.png)When working on ETL flows, its sometimes useful to store information in temporary files as long as you clean those files up when you are finished. Pentaho Data Integration (aka Kettle or PDI) has two steps for deleting file(s) - one handles a single file, and one handles multiple files. Both are in the File Management section of the Design node in the Job designer.
<!-- more -->
### Single files

[![deletefile](http://edpflager.com/wp-content/uploads/2014/04/deletefile-300x100.png)](http://edpflager.com/wp-content/uploads/2014/04/deletefile.png)If you have a single file that you need to delete as part of your workflow and you use the same name over and over every time your job runs, then deleting it is pretty straightforward. For a single file, drag and drop the Delete a file... job entry onto your canvas. Open the step up, and enter a name for it. If the file is created/stored on the computer you are developing on, you can navigate to it by clicking the Browse button. If you are developing on one computer, but the file will be stored somewhere else, you can supply a UNC path to the folder in the format of: \\computernamesharefoldersubfolderfilename. (the folder and subfolder are optional if the file is in the root of the share).  The exact notation will vary depending on whether you are working on Windows or Linux or Mac. Finally, you may choose to fail the step if the file does not exist by checking the box supplied. Click OK and wire up the job with a START and Success job entry, and you are finished.

###  Delete multiple distinct files

[![deletefiles](http://edpflager.com/wp-content/uploads/2014/04/deletefiles-300x215.png)](http://edpflager.com/wp-content/uploads/2014/04/deletefiles.png) On some of the workflows I create, there may be multiple files of a specific file type created, or the date is appended to the file name because the recipient of the output may store them, or there may be a combination of the two. In that case,  you'll want to use the Delete files... job entry in your work flow. Drag and drop the Delete files... job entry onto your canvas. Open the step up, and enter a name for it. Below that there are a couple of check boxes here that you may use on some occasions:

*   Checking the "Include Subfolders" box will delete subfolders as well as the top level folder. This makes it much easier to clean up nested folders.
*   "Copy previous results to args?" will allow you to use filenames that are created as part of a  previous job entry (as part of the result files).

If your workflow is designed to create a file or multiple files with the same names in the same locations repeatedly, click on the FILE button on the right side, and an Open file window will appear. Navigate to where the file is stored (whether its stored locally or somewhere else on your network), select it and click the Open button. Back in the Delete Files... window, click the Add button to move your selection down to the list of files to process. If you have more files to add, repeat the process, navigating to each individual file and clicking Add after each.

### Delete multiple files with a pattern

As I said earlier some of the workflows I create generate temporary files with the same basic name, but they append a date to the end of the file. For example, there may be a file called "testfile20140403.txt" generated on one day and one called"testfile20140404.txt" generated on the next. In order to have a reusable workflow, I need to use the Wildcard box in the Delete Files... process task. You'll notice that the box is labeled Wildcard (RegExp). This is because it the wildcard expressions you enter in this box have to conform to Regular Expression syntax. I won't delve into the intricacies of RegEx here, other than to provide one brief example. If you need help with the syntax, there are numerous websites that can walk you through it. Returning to my example of "testfile20140403.txt" and  "testfile20140404.txt", I would first use the Folder button to locate the folder the files are or will be stored in. This lets me navigate through my local machine, or if you are using a remote machine, you can use the UNC path to get to the folder. Once you have it open in the window, click the Open button to return to the Delete Files... job entry window. The path to the folder should now be entered in the FileFolder box. In the box underneath it, you can now enter your RegEx file name. For the example above, I would enter: ^.\*txt Now click the Add button to move your selection down to the list of files to process. If you have more files to add, repeat the process, navigating to each folder, adding your RegEx and clicking Add after each. Once you are finished, click OK and wire up the job with a START and Success job entry, and your job is done.