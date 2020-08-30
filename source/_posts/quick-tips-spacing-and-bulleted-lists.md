---
title: Quick Tips - Spacing and Bulleted Lists
tags:
  - cookbook
  - howto
  - R Markdown
  - technical
id: '4290'
categories:
  - - Blog
  - - Misc
  - - R
  - - R Markdown Cookbook
comments: false
date: 2018-12-27 12:55:03
---

[![](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)Wrapping up the year with a few simple tips for R Markdown that work with PDF, Word or HTML output. **VSPACE** When you insert a series of blank lines in an R Markdown document and knit the document, the parsing of your document strips out those blank lines. As an example I entered in my file the lines on the left in the image below, and when I Knitted the document, the results were those on the right: [![](http://edpflager.com/wp-content/uploads/2018/12/R-Markdown-spacing-example-300x79.png)](http://edpflager.com/wp-content/uploads/2018/12/R-Markdown-spacing-example.png)
<!-- more -->
If the blank lines are significant in the output, the parsing engine can be forced to insert them for you with a simple command. Here are two examples:

vspace{20pt}  

vspace{1.5in}

The **vspace** command will allow you to specify vertical space in your document. Just include a specific measurement in braces after the command. You can use standard measurement scales like cm (centimeters), mm(millimeters), or in(inches), or web based measurements like pt (points), em (current font size), or px(pixels). **HSPACE** If you want to enter horizontal space, you can use the **hspace** command, again with the various scales above. It can be used for a custom indentation on a paragraph or if you want to position something in a specific location in your document. An example of the second option might be similar to entering this command:

Here there be hspace{2in}DRAGONS

To produce:   [![](http://edpflager.com/wp-content/uploads/2018/12/hspace-example-300x23.png)](http://edpflager.com/wp-content/uploads/2018/12/hspace-example.png) **Bulleted Lists Tips** Its fairly well documented that to include a bulleted list if items in R Markdown, proceed each entry with an asterisk(\*). If after knitting, your bullets display as asterisks then you probably need to precede your list with a blank lime to get them to display correctly. Also if there appear to be blank lines between your items, you may have extraneous characters at the end of your items. Try removing it and Knit your document again to clean it up. Here is an example:

There are two types of output formats in the rmarkdown package: 
Presentations:

\* beamer (this line has extra space) 
\* ioslides
\* powerpoint
\* slidy

vspace{.5cm}
Documents:

\* github
\* html
\* latex
\* md
\* odt
\* pdf
\* rtf
\* word

This code produces this output: [![](http://edpflager.com/wp-content/uploads/2018/12/Bullets-300x220.png)](http://edpflager.com/wp-content/uploads/2018/12/Bullets.png) To fix it, remove the extra space at the end of the "beamer" line. Here is the code for the above examples, in one document:

\---
title: "SimpleTips"
output:
pdf\_document
---

\`\`\`{r setup, include=FALSE}
knitr::opts\_chunk$set(echo = TRUE)
\`\`\`

line 1 - followed by 5 blank lines

line 2

###VSpace

vspace{2em}
The vspace command will allow you to specify vertical space in your document. Typically RMarkdown will ignore blank lines in your document. If you want to explicitly specify space, then this is a good way to do it. Just include a specific measurement in braces after the command.

vspace{2em}

###HSpace

Here there be hspace{2in}DRAGONS


###Bulleted lists
When doing bulleted lists, make sure to have a blank line before the list to ensure that the bullet shows properly. If it shows as an asterick (\*) then you didn't precede your list correctly.

Also if there appear to be blank lines between your items, you may have extraneous characters at the end of your items. Try removing it and Knit your document again to clean it up. Here is an example

There are two types of output formats in the rmarkdown package: 
Presentations:

\* beamer (this line has extra space) 
\* ioslides
\* powerpoint
\* slidy

vspace{.5cm}
Documents:

\* github
\* html
\* latex
\* md
\* odt
\* pdf
\* rtf
\* word