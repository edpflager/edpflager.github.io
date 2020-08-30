---
title: Pentaho Kettle - Create Destination Tables - on the fly
tags:
  - ETL
  - howto
  - kettle
  - PDI
id: '2089'
categories:
  - - Blog
  - - Business Intelligence
  - - Pentaho
comments: false
date: 2014-05-15 03:29:36
---

[![fly](http://edpflager.com/wp-content/uploads/2014/05/fly-300x200.png)](http://edpflager.com/wp-content/uploads/2014/05/fly.png)A quick tip today that can save you a lot of time when using Pentaho Kettle (aka PDI) to move data from one system to another. If you don't have your tables created in your destination system, you can switch between PDI and your database system's management software to create your tables, but there is an easier way. If the account that is used in your database connection in Pentaho has permissions to create tables  on the destination database, you can create new tables from within your transformation workflow!
<!-- more -->
1.  To start, create a new transformation.
2.  Add a Table Input element to your transformation. Double click the element and choose your database connection. Click the Get SQL select statement and find the table you want to copy. When prompted to include the field names, choose Yes.
3.  Add a Table Output destination element and create a hop from the Table Input to the Table Output element.
4.  Double click your destination element and select your destination connection. For your destination table, enter the name for a non-existent table.
5.  Click the SQL button. You'll see a SQL statement to create the destination table. Click EXECUTE to have Pentaho create the table. That's it! Pentaho has created your output table.
6.  Close the Execute results window and then the SQL Editor to return to the Table Output window. Close the Table Output window and run your Transformation to have your new table populated!

Bonus: If your source table changes, you can open the Table Output window and click the SQL button to generate an alter statement!