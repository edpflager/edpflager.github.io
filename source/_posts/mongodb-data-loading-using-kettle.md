---
title: MongoDB data loading using Kettle
tags:
  - ETL
  - kettle
  - PDI
id: '1642'
categories:
  - - Blog
  - - Business Intelligence
  - - Pentaho
comments: false
date: 2013-08-30 15:35:29
---

[![etl](http://edpflager.com/wp-content/uploads/2013/08/etl-300x111.png)](http://edpflager.com/wp-content/uploads/2013/08/etl.png) In this tutorial I will show you how to use [Pentaho Data Integration](http://kettle.pentaho.com/) (aka Kettle or PDI) to load data into a [MongoDB database](http://www.mongodb.org/). Before we get started let's define those two items:

*   **PDI** is an open source tool used to extract data from one source, transform it into other formats, and then load it into a destination. This process is called ETL, and its a big part of my day to day job. We'll use PDI because its free open source software, and because it supports connecting to big data systems.
*   **MongoDB** is an open source document NoSQL database system. Rather than storing data in rows and columns like a traditional database system, Mongo stores information in documents. This allows for quicker retrieval of information, but does require some getting used to how information needs to be formatted.
<!-- more -->
Prerequisites for this tutorial:

*   MongoDB installed locally or on another server you have access to with an empty MongoDB database available.
*   MySQL or MariaDB installed locally or on another server you have access to with the MySQL version of AdventureWorks available from [SourceForge](http://sourceforge.net/projects/awmysql/).
*   Pentaho DI (Kettle) installed locally.

## Pull data from a source table

The first part of the process is to define where the data we want to move is currently located. For our example, we are using the Customer table from the AdventureWorks database.

1.  Open up Pentaho DI and create a new Transformation, and add a database connection to your MySQL database. If you're not sure how to do this, check out my previous tutorial on setting up a Kettle repository. Its very similar.
2.  From the Design tab, drag a Table Input object from the Input node into your workspace. Double click on it to open it. Assign a name to the step (always a good idea for help in fixing bugs). Then from the connection drop down, chose your database connection that you created previously. In the SQL box, type in this statement to retrieve all the columns from the contact table: **SELECT  \* FROM customer**![MySQLtableinput](http://edpflager.com/wp-content/uploads/2013/08/MySQLtableinput-300x199.png)
3.  Click Preview at the bottom of the window, and then click OK in the window where you are prompted for the number of rows to retrieve. A sample of the data you will be working with will be displayed.
4.  If everything looks good, click Close to be returned to the Table Input window, and then OK to be returned to the Transform Editor. At this point we have a step in place to pull records, but they aren't going anywhere.

## Create a data destination

Now we need to setup the destination for the data we are pulling from AdventureWorks.

1.  From the Design Tab in PDI, open the Big Data node, and drag a MongoDB Output object to the workspace. Connect the Table Input object to the MongoDB Output object by holding Shift down while you click and drag from the Table Input to the MongoDB Output (or if you have a scroll wheel on your mouse, try clicking with the wheel and dragging between them).![MongoDB Destination](http://edpflager.com/wp-content/uploads/2013/08/MongoDB-Destination-300x233.png)
2.  Double click on the MongoDB Output object and in the Configure Connection tab, enter the connection information for your MongoDB server. You can use a host name or an IP address with the default port 27017. In my setup, entering a username and password caused me to get an "unable to authenticate error". Leaving those fields blank let me connect.
3.  If you already have a destination database and collection created, you can select them by clicking the Get DBs and Get Collections buttons to populate drop down lists. You can enter a new database and collection name in the appropriate boxes as well, in which case the database and collection will be created on the fly. Check the Truncate Collection button if that is appropriate for you and leave the rest of the options set to their defaults.[![MongoDB Connection](http://edpflager.com/wp-content/uploads/2013/08/MongoDB-Connection-300x216.png)](http://edpflager.com/wp-content/uploads/2013/08/MongoDB-Connection.png)
4.  Click on the second tab at the top left, labeled Mongo Document fields. This window should currently be blank. Click the Get Fields button near the bottom left, and a list of fields will be populated from the Customer table. You can modify these as needed (change the fields names, etc). For our purposes, we'll leave them as is.[![MongoDB Fields](http://edpflager.com/wp-content/uploads/2013/08/MongoDB-Fields-300x98.png)](http://edpflager.com/wp-content/uploads/2013/08/MongoDB-Fields.png)
5.  Click OK at the bottom of the screen, and you'll be back at the PDI workspace. Save your transformation, and then click the RUN TRANSFORM button (or press F9). In the Execute a Transformation window, click the LAUNCH button at the bottom.
6.  If all goes well, the data from the Contact table will be read into PDI and passed into MongoDB. At the bottom of the Kettle window, you will see a multi-tabbed execution results window appear.
7.  Check the Step Metrics tab to see a breakdown of the steps in your process, and  the records that were read and written as part of it. Click the Logging tab and you will see a number of batches that were loaded from the source and pushed to the destination.[![MongoDB Logging](http://edpflager.com/wp-content/uploads/2013/08/MongoDB-Logging-300x124.png)](http://edpflager.com/wp-content/uploads/2013/08/MongoDB-Logging.png)
8.  Connect to your MongoDB server and do a count on the customer collection. The results should equal the value shown at the end of the Logging tab, and the written records value from the Step Metrics tab.![MongoDB Count](http://edpflager.com/wp-content/uploads/2013/08/MongoDB-Count.png)
9.  Congratulations! You have imported your first data set into MongoDB using an ETL tool!