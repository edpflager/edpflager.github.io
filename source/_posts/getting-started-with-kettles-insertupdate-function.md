---
title: Getting started with Kettle's Insert/Update function
tags:
  - ETL
  - kettle
  - PDI
id: '1818'
categories:
  - - Blog
  - - Business Intelligence
  - - Pentaho
comments: false
date: 2014-02-27 16:36:15
---

[![MySQL](http://edpflager.com/wp-content/uploads/2014/02/MySQL-300x133.jpg)](http://edpflager.com/wp-content/uploads/2014/02/MySQL.jpg)When using an ETL tool, you often create a workflow that will run multiple times, picking up new and changed records. Getting just those changes is pretty easy using Kettle's Insert/Update tool. (BTW - Kettle is one component in the Pentaho Data Integration application - PDI for short).

### **Assumptions and requirements**

For this tutorial, I am assuming you have access to a MySQL (or MariaDB) database server. We'll be creating a a sample database based on one originally created by [Fusheng Wang and Carlo Zaniolo](https://launchpad.net/test-db/) at Siemens Corporate Research. For our purposes we only need one table with a small amount of data. Copy the script below and save it as a  SQL file on your system. Run it in MySQL to create the database and populate the table (Yes Production is spelling incorrectly).
<!-- more -->
DROP DATABASE IF EXISTS sample; CREATE DATABASE IF NOT EXISTS sample; USE sample; CREATE TABLE departments ( dept\_no CHAR(4) NOT NULL, dept\_name VARCHAR(40) NOT NULL, PRIMARY KEY (dept\_no), UNIQUE KEY (dept\_name) );

INSERT INTO \`departments\` VALUES ('d001','Marketing'), ('d002','Finance'), ('d003','Human Resources'), ('d004','Produktion'), ('d005','Development');

Once you have it loaded,  do a select on your departments table, and it should return five records. Now copy the lines below into a text file using a basic text editor (Notepad, TextEdit, Notepad++, Gedit, etc) and save it as changes.txt to a location you can reach from within Pentaho. Notice we are adding four departments and correcting the spelling on the Production department.

dept\_no;dept\_name d006;Quality Management d007;Sales d008;Research d004;Production d009;Customer Service

**Getting Started**

1.  Open up Pentaho and create a new database connection to your sample database. Be sure to test the connection to make sure you can access the data.
2.  Create a new transformation, and from the Design tab, under the Input node, drag a Text File Input step onto the canvas.
3.  From under the Output node, drag an Insert/Update step onto the canvas.
4.  Connect the two, by holding down your Shift key and click on the Input step. Drag over to the output step and release your mouse. The results should look like this:![insert-update-transform](http://edpflager.com/wp-content/uploads/2014/02/insert-update-transform-300x97.jpg)

5\. Double click the Text file in put step, and on the first tab - File, click the Browse button and navigate to where you saved the changes.txt file, choosing OK on the file location window to be returned to the File tab. Click the Add button to move the file down to the Selected Files table, like this.

[![inputfilelocation](http://edpflager.com/wp-content/uploads/2014/02/inputfilelocation-300x57.jpg)](http://edpflager.com/wp-content/uploads/2014/02/inputfilelocation.jpg)

 6. At this point, you can click the Show File Content button at the bottom to see what is in the text file. It should look like this:

![showfile](http://edpflager.com/wp-content/uploads/2014/02/showfile-300x186.jpg)

7\. Close the Preview data window, and double click on the Insert/Update icon. We need to populate the information here to tell Pentaho how to handle our incoming data. We need to specify:

*   the database connection: "Sample"
*   the target table: "departments"
*   whether to perform updates or not. Check the box to only add new records to your database.
*   the key(s) to look up the value(s): dept\_no = dept\_no. Because we are using the same field names in the input and output streams we can have the same values here. You can use multiple input fields to define your table field, and you can choose different comparison operators as well. Click the Get fields button if you would like to have the fields populated for you. Warning - this will get all of the fields not just primary key ones.
*   In the bottom table, you specify where data coming in from your source gets written to in your destination. If you have a lot of fields in your tables, use the Get update fields button to populate the table. You can always delete items. If you want fields to be updated when changed data comes in, make sure the Update column in this table is set to Y. Change it to N for those fields you don't want to change.
*   The results should look like this:[![insert-updatewindow](http://edpflager.com/wp-content/uploads/2014/02/insert-updatewindow-259x300.jpg)](http://edpflager.com/wp-content/uploads/2014/02/insert-updatewindow.jpg)

 8. Click OK to return to the canvas. Click the run button to run your transformation, and  if you did everything correctly, you should see these results: [![transform_results](http://edpflager.com/wp-content/uploads/2014/02/transform_results-300x24.jpg)](http://edpflager.com/wp-content/uploads/2014/02/transform_results.jpg) Notice that the Input was five rows, but the Output only says 4. The difference is in the Updated field, where there is one record. Pentaho added four new records to your table, and updated one. Verify the results by switching back to MySQL and doing a Select on the table. You should see the four new departments, and the updated Production department with the correct spelling. If your source database has a timestamp field, cast it as a date or a time field to be able to update it in your destination table