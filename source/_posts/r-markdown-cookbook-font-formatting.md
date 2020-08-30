---
title: Font Formatting - Typefaces
tags:
  - Big Data
  - cookbook
  - guides
  - How-to
  - howto
  - R Markdown
  - technical
id: '3871'
categories:
  - - Blog
  - - Business Intelligence
  - - Misc
  - - R
  - - R Markdown Cookbook
comments: false
date: 2018-09-11 19:15:45
---

[![](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png) First a little background - I've been working with R - the free open source statistical computing and graphics software for about a year, exploring various libraries and add-ons, trying to get a better understanding of it. This was spurred on by a couple of things. First, in SQL Server 2016, Microsoft incorporated R into their database system, allowing you to use R with the SQL Server Management Studio for a variety of purposes. And second, Power BI from Microsoft also incorporates R as a data source, allowing you to massage and manipulate data before passing it to Power BI, and you can use it to develop custom visualizations within Power BI. Because I take notes while I learn, I wanted some way to organize handy tips and tricks and show the results. I first experimented with Jupyter, but then I discovered R Markdown. For a full description of what R Markdown is and what its capable of, check out [this article](https://rmarkdown.rstudio.com/articles_intro.html) from RStudio, the developers of R Markdown.  With this library you can create documents (Word, PDF or HTML) within the R Studio IDE to show your code, and the results of the code in a single document. As a way to save my notes while working with R, I have been working with R Markdown, and I will pass on a few of those tips as part of a series: R Markdown Cookbook.
<!-- more -->
By default, the RMarkdown PDF option uses a Times New Roman font. Its very old-school and I believe its used because a lot of R users are academics and it looks like the preferred format for a lot of R documentation. Or it could be that a fixed width font is preferable for academic journals. Regardless, IMHO, the output looks dated and I wanted to use a different font overall in my RMarkdown PDFs. Keep in mind that from a UX perspective, its best to limit your document to two fonts. This gives it a more professional appearance. To change the default font, in a new RMarkdown document add these lines to your YAML header, under the pdf\_document line (watch the spacing!):

output:
   pdf\_document:
     latex\_engine: xelatex
mainfont: font name (like Calibri Light)

To have just the R code use a different font, add this line:

monofont: font name (Arial)

You can also change the font size to one of three options (10pt, 11pt or 12pt) in PDF output. This limitation is due to standard latex classes only accepting those three. To set this option, add this line to your YAML header:

fontsize: 12 pt

BTW - There is away around this that I will cover in a future post. If you like, you can combine all of these, resulting in this YAML header:

\---
title: "Font test"
output:
   pdf\_document:
      latex\_engine: xelatex
mainfont: Calibri Light
monofont: Arial
fontsize: 10 pt
---

  The finished result once you run KNITR will look like this: [![](http://edpflager.com/wp-content/uploads/2018/09/FontOutputSample-1024x510.png)](http://edpflager.com/wp-content/uploads/2018/09/FontOutputSample.png)