---
title: Wait for a File in Pentaho Kettle - Control Flow components - Part 2
tags:
  - ETL
  - How-to
  - howto
  - kettle
  - technical
id: '2727'
categories:
  - - Blog
  - - Linux
  - - Pentaho
comments: false
date: 2015-05-02 19:07:51
---

[![](http://edpflager.com/wp-content/uploads/2015/04/clockwait-300x225.jpg)](http://edpflager.com/wp-content/uploads/2015/04/clockwait.jpg) In my [last post](http://edpflager.com/?p=2716) I wrote about checking for the existence of a file as a control mechanism in a Pentaho Data Integration (aka Kettle) job. This time around, I look at a similar component in Kettle, but this one waits for a file for a specified time. It will make repeated checks to see if the file has appeared, pausing between each check. While the two components have similar functionality, they also have some marked differences:

*   "File Exists" is most useful at the beginning of a job flow, and the "Wait for File" process can be useful in multiple places within a job flow.
*   "File Exists" can access multiple file systems such as HDFS, Amazon's S3 and the local computer and network and  "Wait for File" can  only access the local computer and network.
<!-- more -->
As an example, you have an ETL process scheduled to run at a specific time and a file is needed about 75% of the way through to complete the job. Because the exact time when the file will be ready isn't known, you can include a Wait for File component in the job that does a periodic check. If the file isn't found, the process will sleep, and then check again, eventually succeeding or timing out. To create an example using the Wait for File component, follow these instructions:

1.  Start Spoon and create a new job.
2.  From the General node in the component panel (the left panel in Spoon), drag a Start and a Success component on to the canvas.
3.  Open the Utility node, and drag an Abort job component to the canvas.
4.  Finally, add a Wait for File component from the File Management node.
5.  Add an unconditional hop between the Start component and Wait for File.
6.  From Wait for File, add a hop to the Success component. Right click the hop, and a menu will appear. Click the Evaluation option to set  “Follow when result it true.”
7.  Add another hop from Wait for File to the Abort component. Right click the hop and from the Evaluation option, click on “Follow when result is false”.![WaitForFile](http://edpflager.com/wp-content/uploads/2015/04/WaitForFile-300x140.png)
8.  Double click the Wait for File component to configure it. You can supply a name for the Job entry (I suggest using the file name you are looking for – i.e. Wait\_for\_file\_name) in the first text box.
9.  The second box is where you enter the file name you want to check for. If you want to access a file local to the machine you are working on, just enter the path to it, preceded by “file:///”  (for example “file:///C:/Users/root/file\_name.txt” on a Windows machine, or on a Mac or Linux machine enter “file:///users/username/file\_name.txt”). Checking for a file on your local network via a UNC is almost as easy, just enter file:////servername/folder\_name/filename.csv). If you’d prefer to browse for the file or aren’t sure of the path to where the file will be stored, just click the Browse button to access the Open File window.(This assumes you already have a file in the location you will be checking).
10.  The "maximum timeout window" setting allows you to specify in seconds how long you want the process to wait. Enter a zero if you want the process to keep cycling until the file appears, with no timeout. Otherwise for example purposes, enter 30.
11.  "Check cycle time" allows you to set how frequently you want to check for the file, in seconds as well. For our example, lets enter 5.
12.  Finally there are three checkboxes you can configure. If the job times out, and you want to treat the timeout as a success, check the first box.
13.  After the file appears, you may want to wait to make sure that the process creating it has completed before your job starts to read it. Mark the File size check box to have the process wait the number of seconds entered in the Check cycle time field. As an example, if you select the File size check box, and have 60 in the check cycle time, once your job sees the file, it will wait an additional 60 seconds before it starts to read it.
14.  The final checkbox is Add filename to result. This option allows you to add filenames to the internal result files of a transformation so that later job entries can use this information.
15.  Once you have configured the various options, click on OK. Your job should look like this:[![WFFJob](http://edpflager.com/wp-content/uploads/2015/04/WFFJob-300x134.png)](http://edpflager.com/wp-content/uploads/2015/04/WFFJob.png)
16.  Save it and run it.
17.  If you specified a file that does not exist, the job will loop through every five seconds checking for the file. When it reaches your timeout value, it will Abort the job, unless you chose the box to treat a timeout as a success.
18.  If you specified a file that does exist, the job should find it on the first iteration and proceed to the Success step.