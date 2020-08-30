---
title: Hyperlinks in R Markdown HTML output
tags:
  - cookbook
  - howto
  - R Markdown
id: '4161'
categories:
  - - Misc
  - - R
  - - R Markdown Cookbook
comments: false
date: 2018-11-15 17:22:41
---

[![](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)R Markdown documents can be generated as PDF, HTML or Word documents. Often times you may want to include hyperlinks in your document to go to a specific webpage, to open an email program, or to even just jump to another location in the document. Lets look at how to add these types of hyperlinks to each document output type. In this post, I'll be covering HTML Hyperlinks and I'll cover [Word](http://edpflager.com/2018/11/21/hyperlinks-in-r-markdown-word-output/) and [PDF](http://edpflager.com/2018/11/23/hyperlinks-in-r-markdown-pdf-output/) in subsequent posts. By far, adding hyperlinks when generating an HTML document is easy in R Markdown. An inline hyperlink to a website in your document is enclosed in less-than and greater-than tags, like this: <http://edpflager.com>.
<!-- more -->
The resulting HTML doc then resembles this:

[![](http://edpflager.com/wp-content/uploads/2018/11/html-hyperlink.png)](http://edpflager.com/wp-content/uploads/2018/11/html-hyperlink.png)

If you want to hide the hyperlink there are a couple of ways to do it, enclose the text you want to substitute for your hyperlink in brackets and follow it with the hyperlink tag in parentheses right after it. The other method is a more traditional HTML approach, enclosing the link inside a set of greater-than and less-than tags, using an href call with the hyperlink, and nesting your substitute for the hyperlink before a closing /a tag. A simple example to illustrate both looks like this with the resulting generated HTML putput:

Hide your \[hyperlink\](<http://edpflager.com>) like this or
like <a href="http://edpflager.com">this</a>[![](http://edpflager.com/wp-content/uploads/2018/11/html-hidehyperlink-1.png)](http://edpflager.com/wp-content/uploads/2018/11/html-hidehyperlink-1.png)

To include a hyperlink to open an email program (assuming the reader has one installed), we can use either of the methods described above. The code to put in your R Markdown document looks like this:

\[email me\](<mailto:test@foobar.com?subject=Hyperlinks>) or 
<a href="mailto:test@foobar.com?subject=Hyperlinks">email me</a>[![](http://edpflager.com/wp-content/uploads/2018/11/html-emailhyperlink.png)](http://edpflager.com/wp-content/uploads/2018/11/html-emailhyperlink.png)

Finally, to include a hyperlink to a point further in the document, you can use the HTML anchor tag by placing your hyperlink at some point in your document and then defining the landing location for the tag in another part of the document. The initial hyperlink is similar to the ones discussed above, but instead of including a full URL, you use a pound sign (#) and a unique name for the anchor. Then later in the document where you want the hyperlink to jump to, you define the landing link with the id= tag. Let's look at an example:

<a href="#land-here">Jump to the landing spot.</a>

OTHER TEXT GOES HERE

<a id="land-here">Landing spot</a>

In my sample R Markdown document, I pasted several paragraphs of Lorem Epsum text to separate the two tags. Also of note:

*   You don't need to include actual text as part of your landing spot. If you have the opening and closing tags next to each other for the landing spot, it still works fine.
*   Its generally a good idea to format your landing spot as a section header, but its not required.

Here is the code for my sample document:

\---
title: "Hyperlinks-HTML"
output: html\_document
---

\`\`\`{r setup, include=FALSE}
knitr::opts\_chunk$set(echo = TRUE)
\`\`\`
<a href="#land-here">Jump to the landing spot.</a>

An inline hyperlink to a website in your document is enclosed in less-than
and greater-than tags, like this: <http://edpflager.com>

Hide your \[hyperlink\](<http://edpflager.com>) like this or 
like <a href="http://edpflager.com">this</a>

Provide an email hyperlink like this:

\[email me\](<mailto:test@foobar.com?subject=Hyperlinks>) or 
<a href="mailto:test@foobar.com?subject=Hyperlinks">email me</a>

Lorem ipsum

<a id="land-here">Landing spot</a>