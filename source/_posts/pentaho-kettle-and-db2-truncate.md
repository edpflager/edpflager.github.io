---
title: Pentaho Kettle and DB2 - Truncate (updated)
tags:
  - Big Data
  - ETL
  - How-to
  - kettle
  - PDI
id: '2214'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
  - - Pentaho
comments: false
date: 2014-07-19 12:11:32
---

[![truncatedisk](http://edpflager.com/wp-content/uploads/2014/07/truncatedisk-150x150.jpg)](http://edpflager.com/wp-content/uploads/2014/07/truncatedisk.jpg)**Updated to cover error on truncating empty table** Recently while working on a data transformation to move records on a regular basis from a PostgreSQL database system to a DB2 mainframe system, I ran across an interesting problem.  The scope of the project called for a complete refresh of each of the tables rather than just updating old records and inserting new ones, so I would need to clear each table out prior to writing the refreshed records. Normally I would use the Truncate Table option in the Table Output step to handle this, but  I found that it caused an error in the workflow. Apparently this problem is due to DB2 not supporting the Truncate SQL command.
<!-- more -->
If you have this problem, here is a fix to resolve it.

1.  Prior to the workflow step where you populate the table with new data, add a transform and drag an Execute SQL task on to the canvas.
2.  Double click the Execute SQL Script and configure it like this. Add a one line command DELETE FROM <schema.tablename>; and select the Execute as a Single Statement option. ![SQLScript](http://edpflager.com/wp-content/uploads/2014/07/SQLScript-300x203.png)
3.  Save your workflow and run it to test.

There are a couple of gotchas with this process:

*   Be sure the account you are using to access DB2 has permissions to Delete from the table you are working with.
*   If the table you are attempting to truncate is empty, it will generate an error in DB2. To get around this, modify your transform to get a row count of the table and use a switch statement to run your truncate script or go to a Dummy statement, depending on whether or not the table is already empty. The transform should look like this:[![image001](http://edpflager.com/wp-content/uploads/2014/07/image001-300x119.png)](http://edpflager.com/wp-content/uploads/2014/07/image001.png)

PENTAHO is a registered trademark of Pentaho, Inc.