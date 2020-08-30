---
title: Using Inline Code
tags:
  - cookbook
  - How-to
  - howto
  - technical
id: '3961'
categories:
  - - Blog
  - - Misc
  - - R
  - - R Markdown Cookbook
comments: false
date: 2018-09-17 12:46:51
---

[![](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)When working with R Markdown, there may be times where rather than having a chart or figure set off from the rest of your document, you may want to have the results of an R statement embedded directly within a line or paragraph of text. So, for example, if you were working on an HTML document using a dataset that changes over time, you could write your text and include R Markdown inline code. The syntax is similar to a normal Code chunk but stripped down a bit. You use a tick mark around your code, and an r indicator followed by your code. This example displays the current time in your output document:

'r Sys.time()\`.
<!-- more -->
If you want to wrap that in text, you could have a line like this:

As of now \`r Sys.time()\`, the mean of the Car Speed data was: \`r mean(cars$speed)\`

Which generates results like this:[![](http://edpflager.com/wp-content/uploads/2018/09/inline-code-with-text.png)](http://edpflager.com/wp-content/uploads/2018/09/inline-code-with-text.png) As I said earlier this is very handy when you are generating an HTML document, because both the system time and the result of the mean statement will be retrieved when the webpage is refreshed. It does have some use in PDF output, displaying when the document was generated and the mean value at that time.