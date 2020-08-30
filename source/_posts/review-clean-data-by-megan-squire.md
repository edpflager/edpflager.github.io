---
title: 'Review: Clean Data by Megan Squire'
tags: []
id: '2817'
categories:
  - - Blog
  - - Business Intelligence
comments: false
date: 2015-06-24 07:00:01
---

## [![CleanData](http://edpflager.com/wp-content/uploads/2015/06/CleanData-243x300.png)](http://edpflager.com/wp-content/uploads/2015/06/CleanData.png)CLEAN DATA by Megan Squire Grade: A

One of the most time consuming, but also most important aspects of a data analysis project is cleaning the source data you are using. In a [New York Times](http://www.nytimes.com/2014/08/18/technology/for-big-data-scientists-hurdle-to-insights-is-janitor-work.html?_r=0) piece last year, this process was called data wrangling or data janitor work, and it was stated that 50 to 80 percent of a data scientist's time will be spent on it.  The challenges can involve anything from removing extraneous characters to misspelled words to incorrect dates to any of hundreds of other issues. But the thing they all have in common is that before fixing the issue you have to understand what the issues are.

Megan Squire, professor of Computing Sciences at Elon University in North Carolina, attempts to break through some of these problems with a series of techniques to make the process more manageable in her book [**Clean Data** from Packt Publishing](https://www.packtpub.com/big-data-and-business-intelligence/clean-data).  Aimed at data scientists, this book also can benefit those who work in the ETL end of the process, since there is considerable cross over in functions.
<!-- more -->
Starting with basic issues like file types, encoding, null and empty fields, it progresses to using widely available tools like Microsoft Excel, Google Spreadsheets, and Sublime Editor to winding up with  writing custom code in PHP and Python. Specific examples of how to overcome some aspect of data cleansing that you may be faced with are presented and some good rules to follow are emphasized, such as document the steps you took.

One of the more interesting aspects for me in this book was the chapter on cleaning data from PDF files. Designed by Adobe to allow sharing of documents across multiple platforms, anyone who has tried to copy data out of PDF files can tell you its often its not an easy process. Luckily, the bulk of the time I have had to do it has involved a page or two at most, so cleaning up the data hasn't been too time consuming. Dr. Squire presents several techniques for getting this information out of a PDF depending on how the information was originally formatted, and for longer extractions I can definitely see a benefit.

The biggest issue for me with this book is the reliance on writing your own code for many of the projects. While I would expect custom SQL code, since by its very nature Cleaning Data involves databases, I tend to steer away from customized coding whenever possible. By relying on external programming languages,  it becomes more difficult to share with others, since they may be unfamiliar with the programming language. But, while I am not a huge fan of using custom programming to perform ETL functionality, I understand it has its place. And in the real world the quickest technique to overcome the existing problem is usually the one that will be used.

While not perfect, this is definitely a worthwhile addition to the fledging data wrangler's bookcase. Real world data sets (Facebook, Twitter, and Stack Exchange to name only a few) are used rather than contrived text book examples, and the techniques can be reused or expanded upon by the reader.