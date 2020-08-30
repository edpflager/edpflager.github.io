---
title: Using MariaDB JDBC with Pentaho Kettle (PDI)
tags:
  - ETL
  - How-to
  - howto
  - kettle
  - PDI
  - technical
id: '2659'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
  - - Pentaho
comments: false
date: 2015-02-14 19:14:53
---

[![Mariadb-seal-shaded-browntext-alt](http://edpflager.com/wp-content/uploads/2015/02/Mariadb-seal-shaded-browntext-alt.png)](http://edpflager.com/wp-content/uploads/2015/02/Mariadb-seal-shaded-browntext-alt.png)A few weeks ago I got an email from reader Zachary Nielsen asking some questions about using the MariaDB JDBC driver with Pentaho Data Integration (aka PDI or Kettle). He had gotten it working as a JDNI option in PDI but wanted to have MariaDB listed as a database option in the database connection window. I looked into a bit, since I had not worked with the MariaDB JDBC connector, and here is what I found. For those unfamiliar with [MariaDB](http://www.mariadb.org), its a fork of MySQL by the original developers of MySQL who had concerns over the acquisition of MySQL by Oracle. MariaDB can be used as a drop-in replacement for MySQL, and works using the MySQL syntax, ports and tools (MySQL Workbench and MySQL JDBC drivers), but additional functionality is also available if you like. The MariaDB team also released a JDBC driver to work in place of the MySQL one that appears to process faster (although the benchmarks are almost two years old - you mileage may vary). In this part of the series, I'll walk through setting up Pentaho DI to use the MariaDB JDBC driver. I'm still working on implementing the driver on a Pentaho ETL server so that part of the series will come later.
<!-- more -->
As initially installed Pentaho Data Integrator has built in support for a number of a different database platforms, from commercial products like Microsoft SQL Server and DB2 to open source platforms like PostgreSQL and of course MySQL. To actually get them to work, you may need to download a JDBC JAR file driver and install it.  Once you have that done, Pentaho will be able to create a connection to the particular database platform. I've covered previously how to get a [MySQL connection working](http://edpflager.com/?p=1622 "Updated: Set up a Kettle repository using MySQL"), but if you refer to use the MariaDB JDBC driver to connect to a MariaDB database instance, the process is a little different, although not difficult.

1.  Download the MariaDB JDBC file from their website [here](https://downloads.mariadb.org/client-java/1.1/). Get the mariadb-java-client-1.1.8.jar file and copy it to the /data-integration/lib folder for Pentaho.
2.  From the /data-integration/simple-jndi folder, open the jdbc.properties file with a text editor. At the end of the file append these five lines, editing them for your database instance. In my case, the database schema I created was called "books" and my server IP address was the one specified. My MariaDB instance was using the default port of 3306 : **        Books/type=javax.sql.DataSource** **        Books/driver=org.mariadb.jdbc.Driver** **        Books/url=jdbc:mariadb://192.168.255.134:3306/books** **        Books/user=edpflager** **        Books/password=password**
3.  Once you have made those additions, save the file.
4.  If you have PDI open, close it and restart it. Start a new transformation, and then start to create a new database connection from the View tab on the left side of the PDI window. Supply a name for the connection (Books in my case), and then scroll through the list of connection Types, choosing Generic database. At the bottom of the screen, switch the Access selection to JNDI. Finally click the Test button at the bottom of the screen and you should be rewarded with a successful connection message. You can also click the Explore button to look through the schema on the database server.[![connector](http://edpflager.com/wp-content/uploads/2015/02/connector-300x257.png)](http://edpflager.com/wp-content/uploads/2015/02/connector.png)
5.  That is it. You can now use the MariaDB driver within your PDI jobs and transformations.

Unfortunately, adding MariaDB as an option in the Connection Type selection is much more involved. From my understanding, each of the options in that panel is tied to underlying application code that defines how to make a connection to a particular database type. In order to add MariaDB as a listed database, the source code for PDI would need to be downloaded and altered to include MariaDB with the additional application code for connecting to MariaDB via ODBC and JDBC. Suggesting that to Pentaho may also get it included in a future version as well.