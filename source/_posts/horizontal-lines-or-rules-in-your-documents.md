---
title: Add Horizontal Lines or Rules
tags:
  - guides
  - How-to
  - howto
  - R Markdown
  - technical
id: '4088'
categories:
  - - Blog
  - - R
  - - R Markdown Cookbook
comments: false
date: 2018-10-18 03:27:53
---

[![](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png) A very quick tip that works if you are creating PDF, Word or HTML output from your R Markdown code. If you want to insert a horizontal line (or rule) in your document, you can use three (or more) asterisks (\*\*\*), three or more underscores (\_\_\_) or three or more hyphens (---). Just remember to have a blank line before and after them so the R Markdown engine can parse the code correctly. Here is a code example:
<!-- more -->
\---
title: "Horizontal Rules"
output:
  pdf\_document: default
  html\_document:
    df\_print: paged
  word\_document: default
---

\`\`\`{r setup, include=FALSE}
knitr::opts\_chunk$set(echo = TRUE)
\`\`\`

\*\*\*

Three (or more) asterisks inserts a horizontal line 

\_\_\_

and so does three (or more) underscores

---

as well as three (or more) hyphens. Just remember to have a blank line 
before and after them so the R Markdown engine can parse the code correctly.

The resulting document looks like this: [![](http://edpflager.com/wp-content/uploads/2018/10/HorizontalLines-1024x382.png)](http://edpflager.com/wp-content/uploads/2018/10/HorizontalLines.png)