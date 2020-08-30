---
title: Add Apache Spark to Cloudera
tags:
  - CDH
  - Hadoop
id: '1877'
categories:
  - - Big Data
  - - Blog
  - - Business Intelligence
comments: false
date: 2014-03-20 15:32:18
---

[![Spark-logo-192x100px](http://edpflager.com/wp-content/uploads/2014/03/Spark-logo-192x100px.png)](http://edpflager.com/wp-content/uploads/2014/03/Spark-logo-192x100px.png)Just a quick note today. A couple of weeks ago, the [Spark](http://spark.apache.org/) project over at Apache graduated to a top-level project and it can now be integrated into your Cloudera environment very easily! Spark is a Hadoop integrated in-memory data analytic framework that uses HDFS (the Hadoop file system) to run programs 100x faster than MapReduce. Speed when using disk isn't quite as fast, just a 10x faster claim than HDFS. It supports a number of different programming languages (Python, Java, Scala), can be used with UC Berkeley's Shark application to see those same speed increases with Hive, and it can read from HBase and Cassandra data sources as well. If you'd like to add Spark to your existing Cloudera cluster, head on over to [Cloudera's website](http://www.cloudera.com/content/cloudera-content/cloudera-docs/CM4Ent/latest/Cloudera-Manager-Installation-Guide/cmig_spark_installation_standalone.html) for instructions on how to install it.