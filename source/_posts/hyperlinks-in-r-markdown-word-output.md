---
title: Hyperlinks in R Markdown Word output
tags:
  - howto
  - Pandoc
  - R Markdown
id: '4178'
categories:
  - - Misc
  - - R
  - - R Markdown Cookbook
comments: false
date: 2018-11-21 12:14:13
---

[![](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png) In my [last post](http://edpflager.com/2018/11/16/html-hyperlinks-in-r-markdown-pdfs/), I covered adding hyperlinks to R Markdown files for HTML output. This time, I'll look at adding similar hyperlinks to Microsoft Word output from R Markdown documents. In my experience, many people are not aware that you can have hyperlinks in Word documents, but they do provide some useful functionality. Although there isn't as many available options, the main ones are available: link to a website, and link to sending an email. You can show the destination webpage URL or replace it with alternative text and have a tooltip that pops up with the actual link when you hover over the hyperlink. When setting up an email hyperlink, you can include a subject for the email, and also a pre-filled message body if you like! The only option I have not been able to replicate from the HTML options was the landing page option (but I'm working on it, and I will update this page if I find a solution)
<!-- more -->
As with HTML output, in Word output, an inline hyperlink to a website in your document can be enclosed in less-than and greater-than tags, like this: <http://edpflager.com> or can simply be typed out: http://edpflager.com. The resulting Word doc then resembles this:

[![](http://edpflager.com/wp-content/uploads/2018/11/Word-hyperlink-300x64.png)](http://edpflager.com/wp-content/uploads/2018/11/Word-hyperlink.png)

To hide your URL and display plain text instead, enclose the alternative text in brackets, and follow it with your URL inside of parentheses. The code would then look like this:

Hide your \[hyperlink\](<http://edpflager.com>) like this and provide 
a tooltip instead. Whatever is contained in the preceding brackets becomes
 the hyperlink.

[![](http://edpflager.com/wp-content/uploads/2018/11/Word-hidehyperlink.png)](http://edpflager.com/wp-content/uploads/2018/11/Word-hidehyperlink.png) Finally, if you want to include a hyperlink in your Word document to  create an email, the syntax is identical to one I used in the HTML example.

\[email me\](<mailto:test@foobar.com?subject=Hyperlinks 
work in Word?body=This website is awesome>)

Using this syntax will result in the words "email me" being formatted as a hyperlink in your output Word document. Clicking that link will launch your email program if one is installed and start a new email message with the specified recipient, subject line and message body! And here is my code for this post:

\---
title: "Hyperlinks-Word"
output:
   word\_document: default
---

\`\`\`{r setup, include=FALSE}
knitr::opts\_chunk$set(echo = TRUE)
\`\`\`

An inline hyperlink to a website in your document is enclosed in less-than and greater-than tags, like this: <http://edpflager.com>

Hide your \[hyperlink\](<http://edpflager.com>) like this and provide a tooltip instead. Whatever is contained in the preceding brackets becomes the hyperlink.

The HTML version doesn't work for Word output: like <a href="http://edpflager.com">this</a>

Provide an email hyperlink like this:
\[email me\](<mailto:test@foobar.com?subject=Hyperlinks
work in Word?body=This website is awesome>)