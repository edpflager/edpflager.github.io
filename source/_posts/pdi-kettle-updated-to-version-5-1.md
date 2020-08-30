---
title: PDI - Kettle updated to version 5.1
tags:
  - ETL
  - external article
  - HortonWorks
  - PDI
  - technical
id: '2169'
categories:
  - - Blog
  - - Business Intelligence
  - - Pentaho
comments: false
date: 2014-06-27 16:01:35
---

[![pdi51](http://edpflager.com/wp-content/uploads/2014/06/pdi51-259x300.png)](http://edpflager.com/wp-content/uploads/2014/06/pdi51.png)While I was lounging around the back roads of Texas last week, Pentaho, Inc. was busy - releasing version 5.1 of Kettle in both Enterprise and Community editions. I've downloaded and installed the CE version on my laptop, and plan to put it through its passes this weekend. [Pentaho's website](https://help.pentaho.com/Documentation/5.1/0T0/0Z0/000) lists the improvements in more detail but here are some of the highlights that look interesting to me. There are new Big Data features including:
<!-- more -->
*   support for current versions of a couple of NoSQL platforms - MongoDB (2.6) and Cassandra (2.0),
*   improved YARN (Hadoop v2) functionality allowing PDI to interact with  more recent versions of several Hadoop platforms Cloudera (version 5), MapR (version 3.1) and HortonWorks (version 2.1) as well as with over a dozen other Hadoop platforms
*   and the ability to execute transforms on YARN clusters in parallel using the data nodes of the Hadoop clusters, rather than processing the data on the PDI box. This should provide a large performance boast since the data will be processed where it resides, rather than pulling it across the network.

Documentation is being improved! This faces one of the biggest issues I see with open source software. While functionality of OSS many times is head and shoulders above its commercial counterparts, the poor quality or complete lack of documentation turns off a lot of potential users. Lets hope it continues! Security features have been added, and one of the biggest is the ability to grant or restrict permission to run transformations/jobs by role. In many large scale environments subject to Sarbanes Oxley or other regulations, developers may not be given the ability to run code on production data systems. Now Pentaho has implemented that level of security for ETL developers as well. There are quite a few other improvements, that I won't go into here, but do check out Pentaho's website for more information.