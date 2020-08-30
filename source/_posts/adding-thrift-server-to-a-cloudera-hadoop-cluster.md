---
title: Adding Thrift server to a Cloudera Hadoop Cluster
tags:
  - Big Data
  - CDH
  - Hadoop
id: '1764'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2014-02-10 16:22:16
---

[![thriftserver](http://edpflager.com/wp-content/uploads/2014/02/thriftserver.jpg)](http://edpflager.com/wp-content/uploads/2014/02/thriftserver.jpg)This is a continuation of my series on setting up a Hadoop Cluster using Cloudera's distribution. When using the HBASE application, or Impala, you may receive errors about the THRIFT service being unavailable. From what I have found this is because Cloudera doesn't install the THRIFT service as part of the automated installation.
<!-- more -->
Before we dig into getting Thift running on your Hadoop cluster, lets take a minute to understand what Thrift is designed to be: Its a light weight bridge across various programming languages allowing applications built with those languages to communicate and pass data. Developed initially [by Facebook](http://thrift.apache.org/static/files/thrift-20070401.pdf), it has been open sourced and is now housed in the Apache Software Foundation library. Because the applications that make up the Hadoop ecosystem were created with many different computer languages, Thrift allows those applications to communicate and pass data back and forth.

## Installation

To install it, first download and copy the repository file for your distribution from this [page](http://www.cloudera.com/content/cloudera-content/cloudera-docs/CDH4/latest/CDH4-Installation-Guide/cdh4ig_topic_4_4.html#topic_4_4_2_unique_1) and follow the instructions for installing it. Then install the service using your distributions installation command. For CentOS, I entered **yum install hbase-thrift** as root. It will take several minutes to install while all the dependencies are linked in. Once you are returned to the command prompt, enter the following to start the service. **sudo service hbase-thrift start** Now restart your HBase server, and you should be good to go.