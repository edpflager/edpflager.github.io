---
title: Setup a Single-node Hadoop Yarn machine using CDH5 – Part 3
tags:
  - CDH
  - Cloudera
  - Hadoop
  - How-to
  - howto
  - install
  - technical
  - YARN
id: '2511'
categories:
  - - Big Data
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2014-10-10 18:03:06
---

This is part 3 of a series about setting up a single-node Hadoop Yarn system for sandbox use. Part 1 was [here](http://edpflager.com/?p=2475 "Setup a Single-node Hadoop Yarn machine using CDH5 – Part 1"), and part 2 [here](http://edpflager.com/?p=2490 "Setup a Single-node Hadoop Yarn machine using CDH5 – Part 2"). I have another series for using MapReduceV1, which is [here](http://edpflager.com/?p=1945 "Setup a Single-node Hadoop machine using CDH5 and HUE – Part 1"). I’m hoping to keep this series in a similar order as the original set of articles, and will deviate only when necessary. All the content here is based on the [Cloudera](http://www.cloudera.com/) documentation, but I’ve modified it to be easier to follow for setting up a pseudo cluster and added additional content where necessary. Please be careful when copying lines from these articles to paste into Hadoop config files or a terminal window. I have found that the double hyphen characters used in the comment lines may copy over as a long hyphen instead. This is likely to cause issues when attempting to run the various components. This entry is fairly short, but next time I'll delve into installing HUE.
<!-- more -->
### **Snappy configuration**

Snappy is installed as part of the initial Hadoop installation. Its a compression library used by MapReduce, Hive, HBase, Sqoop and Pig and can make applications run faster without any changes to the application code.

1.  This is one of the deviations between setting up V1 and v2, To enable Snappy for MapReduce v2, from a command line as root, edit:**/etc/hadoop/conf/mapred-site.xml** and add these sections to the file before the final </configuration> tag:  
    <property> <name>mapreduce.map.output.compress</name> <value>true</value> </property> <property> <name>mapred.map.output.compress.codec</name> <value>org.apache.hadoop.io.compress.SnappyCodec</value> </property>
2.  Configurations for other components are enabled in the code to call the specific job.
3.  Restart your server for Snappy to take effect.