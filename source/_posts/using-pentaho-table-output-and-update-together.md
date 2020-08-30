---
title: Using Pentaho Table Output and Update together
tags:
  - ETL
  - How-to
  - howto
  - kettle
  - Linux
  - PDI
  - technical
id: '2449'
categories:
  - - Blog
  - - Business Intelligence
  - - Pentaho
comments: false
date: 2014-09-27 10:38:05
---

Several weeks back, I posted a tutorial on how to use the [Update/Insert function](http://edpflager.com/?p=1818 "Getting started with Kettle’s Insert/Update function") in Pentaho Data Integration (PDI aka Kettle). Recently at work, I had an occasion to revisit the Update/Insert because a workflow using it was not getting all of the updated records. By switching it out with a workflow like that illustrated here, I was able to improve the runtime minutely. and also to rectify the problem.
<!-- more -->
For a setup, I use the same starting database that the Update/Insert tutorial uses. The instructions are reproduced here for ease of use:

##### **Assumptions and requirements**

For this tutorial, I am assuming you have access to a MySQL (or MariaDB) database server. We’ll be creating a a sample database based on one originally created by [Fusheng Wang and Carlo Zaniolo](https://launchpad.net/test-db/) at Siemens Corporate Research. For our purposes we only need one table with a small amount of data. Copy the script below and save it as a  SQL file on your system. Run it in MySQL to create the database and populate the table (Yes Production is spelling incorrectly). DROP DATABASE IF EXISTS sample; CREATE DATABASE IF NOT EXISTS sample; USE sample; CREATE TABLE departments ( dept\_no CHAR(4) NOT NULL, dept\_name VARCHAR(40) NOT NULL, PRIMARY KEY (dept\_no), UNIQUE KEY (dept\_name) ); INSERT INTO departments VALUES (‘d001′,’Marketing’), (‘d002′,’Finance’), (‘d003′,’Human Resources’), (‘d004′,’Produktion’), (‘d005′,’Development’); Once you have it loaded,  do a select on your departments table, and it should return five records. Now copy the lines below into a text file using a basic text editor (Notepad, TextEdit, Notepad++, Gedit, etc) and save it as changes.txt to a location you can reach from within Pentaho. Notice we are adding four departments and correcting the spelling on the Production department. dept\_no;dept\_name d006;Quality Management d007;Sales d008;Research d004;Production d009;Customer Service

##### USING TABLE OUTPUT AND UPDATE

1.  Open up Pentaho and create a new database connection to your sample database. Be sure to test the connection to make sure you can access the data.
2.  Create a new transformation, and from the Design tab, under the Input node, drag a Text File Input step onto the canvas.
3.  Double click the Text file input step, and on the first tab – File, click the Browse button and navigate to where you saved the changes.txt file, choosing OK on the file location window to be returned to the File tab. Click the Add button to move the file down to the Selected Files table.[![inputfilelocation](http://edpflager.com/wp-content/uploads/2014/02/inputfilelocation-300x57.jpg)](http://edpflager.com/wp-content/uploads/2014/02/inputfilelocation.jpg)
4.  At this point, you can click the Show File Content button at the bottom to see what is in the text file. It should look like this:[![showfile](http://edpflager.com/wp-content/uploads/2014/02/showfile-300x186.jpg)](http://edpflager.com/wp-content/uploads/2014/02/showfile.jpg)
5.  Close the Preview Data window and the Text File Input step.
6.  From the Output node drag a Table Output step. Create a normal hop between the two steps.
7.  Open the Table Output step and supply the connection information for the MySQL database that was created earlier in this tutorial. Leave the commit size set to the default value. Make sure the Truncate Table box is unchecked. Check the Specify database fields box.[![tableoutput1](http://edpflager.com/wp-content/uploads/2014/09/tableoutput1-300x251.png)](http://edpflager.com/wp-content/uploads/2014/09/tableoutput1.png)
8.  Switch to the Database Fields tab, and click the Get Fields button on the right side. The Fields to Insert grid should populate with the table name and stream field information.[![tableoutput2](http://edpflager.com/wp-content/uploads/2014/09/tableoutput2-300x250.png)](http://edpflager.com/wp-content/uploads/2014/09/tableoutput2.png)
9.  Click OK to exit the window.
10.  Drag an Update step from the Output node. Start to create a hop between the Table Output step and the Output node, and when the output type menu appears, choose Error Handling of Step.[![stepmenu](http://edpflager.com/wp-content/uploads/2014/09/stepmenu.png)](http://edpflager.com/wp-content/uploads/2014/09/stepmenu.png)
11.  The hop will be created between the two steps, but it will be a dotted red line with a red X appearing on the hop. This denotes its an error handling step.[![errorhop](http://edpflager.com/wp-content/uploads/2014/09/errorhop-300x69.png)](http://edpflager.com/wp-content/uploads/2014/09/errorhop.png)
12.  Open the Update Step, and enter the connection information to the sample database and the departments table. Click the Get fields button next to the key(s) to look up the value(s) grid. Both of the fields from the departments table will be entered. Since the primary key is only the dept\_no field, delete the dept\_name line.
13.  To the right of the Update fields grid, click the Get Update fields button. Again the grid will be populated with both columns from the departments table. Because the update should not be updating the PK, remove the dept\_no field from the grid.[![update](http://edpflager.com/wp-content/uploads/2014/09/update-275x300.png)](http://edpflager.com/wp-content/uploads/2014/09/update.png)
14.  Click OK to close the update window, and save the transformation. Click the RUN button.The transformation should process very quickly, and the metrics window should appear similar to this:[![metrics](http://edpflager.com/wp-content/uploads/2014/09/metrics-300x52.png)](http://edpflager.com/wp-content/uploads/2014/09/metrics.png)
15.  Notice that the Output step shows 4 records were Output and 1 was Rejected. The four new records were added to the deparments table, and one record that was already in the database was passed to the Error handling step. There the record was updated.
16.  Switching back to the MySQL console and doing a Select on the table will verify the results. Four new departments were added, and the spelling for the Production department was corrected.

Pentaho, Kettle and PDI are trademarks of Pentaho LLC.