---
title: 'Pentaho: To Lookup or Not to Lookup'
tags:
  - ETL
  - How-to
  - howto
  - kettle
  - PDI
  - technical
id: '2683'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
  - - Pentaho
comments: false
date: 2015-03-17 18:19:10
---

[![faq](http://edpflager.com/wp-content/uploads/2015/03/faq-300x300.jpg)](http://edpflager.com/wp-content/uploads/2015/03/faq.jpg)Generally when developing an ETL process, if you have to replace a value from a source with a corresponding value, you should use a lookup table. For example, if you were replacing a country abbreviation with the full name of  the country, you could have a simple 2 column table with the abbreviation in one column and the full name in the other. By using a lookup table it becomes very easy to update values, enter new ones, or possibly delete obsolete ones. This also gives you the added benefit of being able to reuse your lookup table if you need to in other places. But what if you have a small group of items (say a dozen or less) that you need to replace? In that case you might want to look at using Pentaho's Value Mapper component.
<!-- more -->
For this example, I am using a simple MySQL database I have, containing books I read, and the month and year when I read them (yes I am a geek). The source table can be created with this code:

CREATE TABLE \`booksread\` (
 \`Title\` varchar(74) DEFAULT NULL,
 \`Author\` varchar(33) DEFAULT NULL,
 \`Rating\` varchar(6) DEFAULT NULL,
 \`MonthRead\` varchar(9) DEFAULT NULL,
 \`YearRead\` varchar(15) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

With the table created, I start a new Pentaho transformation with a connection defined to this database. I drag a Table Input step and a Text file output component to the work area. Between them, I drag a Value Mapper component from the Transform node in the Design panel, and connect them together like this: [![value_mapper1](http://edpflager.com/wp-content/uploads/2015/03/value_mapper1-300x88.png)](http://edpflager.com/wp-content/uploads/2015/03/value_mapper1.png)   The Table Input is fairly generic, with a Select on the various fields.  If you preview the table input, you would see the month name in the source (January, February,etc).The text file output component accepts all of the defaults except the output is written to the desktop. Opening the Value Mapper, in the header area, I identified the source field I want to replace by selecting it from the drop down list.  The target field name can remain empty if you are replacing the existing field, or a new field name can be supplied. Then a default value can be specified in the event that none of the values you define are found. Think of it as an ELSE value.  After that I have entered in the values that I would expect to see along with the value I want to replace them with. For January, replace it with 1, for February replace with 2, and so on. The finished result looks like this:[![value_mapper3](http://edpflager.com/wp-content/uploads/2015/03/value_mapper3-300x209.png)](http://edpflager.com/wp-content/uploads/2015/03/value_mapper3.png)Save the transformation and run it. As long as everything is defined correctly a new text file will be created with a number replacing the month name in the output.