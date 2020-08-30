---
title: Change character encoding with Pentaho Select Values
tags:
  - ETL
  - How-to
  - howto
  - kettle
  - PDI
  - technical
id: '2696'
categories:
  - - Blog
  - - Pentaho
comments: false
date: 2015-04-01 19:08:33
---

[![468319_70689252](http://edpflager.com/wp-content/uploads/2015/04/468319_70689252-300x225.jpg)](http://edpflager.com/wp-content/uploads/2015/04/468319_70689252.jpg)Moving data between different systems often requires converting between different character encoding specifications. If you aren't familiar with the term, it means how characters are stored programmatically. In Latin character based languages like English, there are fewer characters, and they require a smaller amount of code to represent them. When computers had a much smaller amount of memory, processing power and disk space the smaller foot print of the ASCII and Windows 1252 characters sets were widely used to conserve resources. However, in other non-Latin character based languages, there can be a significantly larger amount of characters that make them up. Consequently, more system resources are required to represent them. Today, with larger amounts of data than ever before moving between different counties, moving that information between systems with different character schemes  can result in scrambled data if the underlying encoding isn't taken into account. UTF-8 and UTF-16 encoding schemas are the most prevalent and widely accepted specifications, and should be used in place of the older ASCII and Windows 1252 schemes. Moving data to this format with Pentaho Data Integrator (aka Kettle) can be handled with a transform component called Select Values, although the method to perform this process is somewhat hidden.
<!-- more -->
The example here involves moving some data from an older RDBMS system that is using the Windows 1252 scheme to one using UTF-8.

1.  Start a new transformation in Pentaho with a connection to your source system.
2.  On the workspace, drag out a Table Input component, and define your initial query. For my purposes, I was accessing a table with countries from around the world with their currency names.
3.  Open the Transform node in the Design pallet and drag out the Select Values component.
4.  Attach Table Input to Select Values and double click it. There are three tabs, but you can only use one at a time. If you need to perform operations from two or more tabs, you'll need to string them together in successive steps. The functions are:
    *   Select & Alter allows you to rename fields and also change the Length or Precision of the field.
    *   Remove allows you to remove a field that is no longer needed from the processing stream.
    *   Meta-data allows you to change the information about the fields in the processing stream and includes many of the Select & Alter options.
5.  Click the Meta-data tab, and then click the "Get fields to change" button if you want all of the fields to be added. If you only want one or a few fields to be added, click on the down arrow button on the right side of the field name column  and choose the column you want to add. ![fieldnames](http://edpflager.com/wp-content/uploads/2015/04/fieldnames-300x293.png)
6.  Moving to the right, you can:
    *   rename the field, 
    *   change the type with the length and precision columns (similar to a CAST statement)
    *   switch a binary field to a normal value
    *   supply a different format for the field
    *   indicate if the date format is lenient, change the date locale and date time zone 
    *   indicate if the number conversion is lenient as well, 
    *   change the encoding type of the data, and
    *   make changes to the decimal format, grouping and currency settings
7.  To avoid confusion, its best to rename the field to when changing the encoding. Supply a new name in the rename column.
8.  Click on the down arrow to the right of the encoding column, and you will be presented with a long list of different encoding schemes. Select UTF-8 from the list, and then click OK at the bottom.[![encoding_types](http://edpflager.com/wp-content/uploads/2015/04/encoding_types-233x300.png)](http://edpflager.com/wp-content/uploads/2015/04/encoding_types.png)
9.  Open the table output component and provide the necessary connection information for your output table. Check the Specify database fields box, and then click the Database fields tab.
10.  Click the Get fields button and the fields from the previous step will be populated. If you renamed your column for the encoding change, the new field name will be here. Click OK at the bottom of the window.
11.  Save your transformation and run it. Check the destination database, and the fields' encoding type should now match the new type you selected.