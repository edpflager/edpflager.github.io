---
title: Hadoop and Hive
tags: []
id: '1535'
categories:
  - - Blog
  - - Business Intelligence
comments: false
date: 2013-05-28 10:57:27
---

[![bee_hive_](http://edpflager.com/wp-content/uploads/2013/05/bee_hive_.png)](http://edpflager.com/wp-content/uploads/2013/05/bee_hive_.png)Hadoop is the Apache Foundation's open source MapReduce platform. If you don't know what that means, its OK. Basically, its a Big Data platform based on specifications that Google developed and published. You can run it on one computer, or 10 or 100 or 1000, all talking together.You feed the system a large volume of data, tell it how to organize it (map) and then aggregate it (reduce) to get the results. Its not a great platform for every type of data analysis, but the ones that it is good for work extremely well on it. The biggest problem that I can see people having with Hadoop is the need to know how to program in Java to get it to do any major processing. Hive is a another project in the Hadoop world that lets you use commands similar to those you would with SQL Server or Oracle to manipulate a Hadoop database. Having spent just a little time playing with it, I'm pretty impressed.
<!-- more -->
Using Hive, my one node Hadoop system churned through a 6 million row data set of patent information in a couple of minutes, and provided a list of the most referenced patent IDs with a count of how often they are cited in other patents. Now, I'm not a big fan of the command line, but if you want to work with Hadoop and Hive you have to learn the Linux command line (you can install them on Windows or Mac OSX, but the bulk of development and use is on Linux systems).  There is a lot of development going on to provide GUI interfaces to the various components but the progress is slow. All of these tools are not even at a 1.0 release mark yet, so they are pretty raw, and documentation is very limited. The latest version of Hive comes with a web interface to allow you to access it via a browser, so I've been trying to get that to work. Here is a little tidbit for anyone trying to get the Hive web interface in Linux to work. Enter the command:

hive --service hwi

If you get an error about the hive-site.xml file not being found, change over to the folder where you installed Hive, and go to the conf subfolder. Execute a command to copy the hive-default.xml.template file to hive-site.xml and then try to run the hive --service hwi command again. If you don't get any errors, you can now open a web browser on another machine, and go to: http://<<ipaddress>>:9999/hwi and you'll see the Hive Web Interface screen. (Don't forget the /hwi at the end!). The only problem I've found with this is, you can't access the command line while its running!