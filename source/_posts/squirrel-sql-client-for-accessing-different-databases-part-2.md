---
title: SQuirreL SQL Client for accessing different databases - Part 2
tags:
  - ETL
  - How-to
  - howto
  - install
  - Mac
  - MySQL
  - SysAdmin
  - technical
id: '2825'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2015-06-29 02:38:31
---

[![squirrel2](http://edpflager.com/wp-content/uploads/2015/06/squirrel2.jpg)](http://edpflager.com/wp-content/uploads/2015/06/squirrel2.jpg)In my l[ast post on using SQuirrel SQL Client](http://edpflager.com/?p=2778) (aka Squirrel), I walked through downloading and installing it, and adding a connection to a locally hosted MySQL database. This time, I walk through using Squirrel to manipulate the MySQL database. I'm assuming you have the MySQL test database Sakila installed for this. If not, check out the [documentation](http://dev.mysql.com/doc/sakila/en/sakila-installation.html) at the MySQL website.

##### OBJECTS DISPLAY

1.  Open Squirrel, and from the Aliases tab on the left, double click the entry you setup in Part 1, for the MySQL database. The Connect to window will appear. In the URL field, make sure you have specified a database name. For my setup, the URL reads: **jdbc:mysql://localhost:3306/sakila [![mysql_login](http://edpflager.com/wp-content/uploads/2015/06/mysql_login-278x300.png)](http://edpflager.com/wp-content/uploads/2015/06/mysql_login.png)** 
2.  Enter your password in the appropriate spot and click Connect at the bottom.
3.  On the main work area in the center of the screen, a new window will appear with A LOT going on. In the upper left portion will be a drop down list labeled Catalog. Currently the list should show "sakila" as the selected entry. (Even though all of the databases you have access to on the server will show up in the Object list, in order to switch to a different database, you'll need to select the new database from the Catalog drop down list.)![catalog](http://edpflager.com/wp-content/uploads/2015/06/catalog.png)
4.  Below the Catalog list you'll see two tabs (on Mac OS X you-ll see buttons instead of tabs): **Objects** and **SQL.** If **Objects** is not selected, click on it to make it active. Below Objects on the left will be a search field, and then below that will be an object tree, showing the MySQL databases on the server you are connected to, and at the bottom, a USERS entry. To the right you'll see a set of tabs, a large number of them being reference ones:
    *   FUNCTION names for numbers, strings and time/date and
    *   KEYWORDS - reserved keywords for the particular database
    *   TABLE and DATA TYPES -lists of the various kinds of tables and field types the particular database supports.
5.  Depending on the RDBMS you are connected to you may see system maintenance tabs as well. \[caption id="attachment\_2832" align="aligncenter" width="500"\][![Objects](http://edpflager.com/wp-content/uploads/2015/06/Objects-1024x475.png)](http://edpflager.com/wp-content/uploads/2015/06/Objects.png) SQuirreL SQL Client connected to MySQL database. Objects tab active.\[/caption\]
    
    #### Please Note: None of the information displayed in the Objects area is editable by the user. It is read only.
    
6.  Double click the entry for "sakila" in the tree, to toggle it open and display its sub-tree. The tabs displayed on the right will change and only a few will be shown. The Info tab will give you some very brief Name information about the database. Click on the MySQL Open tables and you will see a list of the tables in the "sakila" database and if the table is in use and/or locked. Click on the MySQL Table Status tab, and you'll get more information about each table and view in the database (MySQL Engine in use, how many rows in the table, etc).
7.  Double click the entry labeled TABLE under "sakila" to see a list of tables within the database. The window on the right will clear, until you select a table, so click on the first one, labeled "actor". \[caption id="attachment\_2837" align="aligncenter" width="500"\][![actor_table](http://edpflager.com/wp-content/uploads/2015/06/actor_table-1024x478.png)](http://edpflager.com/wp-content/uploads/2015/06/actor_table.png) SQuirreL SQL Client connected to MySQL database with table selected\[/caption\]
    *   The first tab, **Info** again provides some basic naming information about the table.
    *   Click the second tab (**Content**) and you can see the first 100 records in the table.
    *   **Row Count** shows you how many records are in the table.
    *   **Columns** provides information on the fields in the table.
    *   There are several tabs showing information about Primary Keys and Exported and Imported Keys (Foreign Key relationships).
    *   Several other tabs are also present, covering various settings and configurations of the database. Depending on the RDBMS you are connected to, what you see may vary.
8.  Below the TABLE entry in the Object list on the left, there is a VIEW entry. Double click that to see the VIEWS that are part of the database. In the "sakila" database there are seven of them, and clicking on each will update the window to the right with information about them. Of special interest is the last tab, labeled **Source.** Clicking on this will show the query that defines the view (very handy!)
9.  Below the VIEW entry, you will see one labeled PROCEDURE. Double clicking on this will show you the six Stored Procedures in the "sakila" database. Clicking on the name of any of them will provide information to the right on them. As with the VIEW section, you can see the source code for the Stored Procedure.
10.  Finally, below PROCEDURE, you will see an option labeled UDT for User Defined Types. If there are any UDTs defined in your database, they will display here. (Sakila has none).
11.  That's the Objects side of the display!
<!-- more -->
##### SQL Display

In the first part of this article we walked through the OBJECTS side of Squirrel. The other tab (or button) available at the top of the MySQL window in the SQUirreL CLient is the **SQL** one.

1.  Click on the **SQL** button or tab now. Compared to the **Objects** area, the work area here is spartan!
2.  There are a row of icons across the top with tool tips. They are broken down into functional areas, such as dealing with the underlying database, file manipulation, query shortcuts, and result handling. Below that is a drop down list, that will be empty if this is the first time you've used SQuirreL SQL Client. This is the Query History drop down. Queries you enter into the builder window below it will be cached and when you exit SQuirreL they get written to a file for retrieval the the next time you start the client.![QueryWindow,png](http://edpflager.com/wp-content/uploads/2015/06/QueryWindowpng-1024x563.png)
3.  To the right of the history drop down is a down arrow to copy the selected query to the query editor. Obviously if nothing has been entered, there is nothing to copy at this point! Next to that is the Open SQL History button, which will open a Window where you can search for a specific query if you have a lot of history. **You should note there is currently no option to delete your query history within the application. In order to remove history, delete the sql-history.xml file from the home/.squirrel-sql folder. It will be recreated the next time you close SQuirreL SQL Client.**
4.  Positioned next (a bit awkwardly) is a checkbox. This applies to the Limit Rows drop down list. If you have **Limit Rows** selected, and the box checked, your query return at most the number of records specified in the field to  the right of the **Limit Rows** button. So as an example, if you are querying a table with 10,000 records, and have limit rows set to 100, it will only return 100 rows of data.
5.  The **Limit Rows** drop down list has a second option: **Read On, Block Size.** With this option selected, the client will read a set of records in the number specified, and will pull in the next set of records when you scroll through the results. Lets try an example. Set the drop down to **Read On, Block Size**. and enter 30 in the text box. In the query window, enter this query: **SELECT \* FROM actor;** and then click the button that looks like a runner to execute the query (Circled in red in the screen capture below).[![ReadOn](http://edpflager.com/wp-content/uploads/2015/06/ReadOn-1024x630.png)](http://edpflager.com/wp-content/uploads/2015/06/ReadOn.png)
6.  The result set of 30 records will appear at the bottom of the window, and a message will show in the middle that the results are limited to 30 records. Scroll through the results past the 30th record, and the next 30 will appear. Neat!
7.  Along with the results, you also have several other tabs that appear as well:
    *   MetaData - shows information about each of the fields returned (data type, scale. precision, nullable, etc).
    *   Info - displays the query along with the elapsed time to execute it.
    *   Overview/Charts shows information on how the records are statistically distributed. Hover your mouse over each block and it will provide a tooltip with the information. You can change the number of groups the data is sliced into by changing the value in the Max Intervals drop down list. Click the Chart button to see several options for visualizing the distribution of the data, and click the Open Chart Window button once you have defined your visualization.[![overview](http://edpflager.com/wp-content/uploads/2015/06/overview-1024x329.png)](http://edpflager.com/wp-content/uploads/2015/06/overview.png)
    *   Rotated table is the last option, and it shows a cross tab like view of your data with each column being one record. If you have a scroll wheel on your mouse, you can use that to scroll through the records, otherwise just use  the slider on the screen.

That's my brief tour of the SQuirreL SQL Client tool! If you need to connect to multiple database platforms, or the database platform doesn't provide a query tool for your operating system, be sure to give it a try. I will say that I have been able to connect to MySQL, SQL Server, and DB2 iSeries with this tool. If you have any questions on how to use it, please let me know, and I'll try to help.