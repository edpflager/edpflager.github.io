---
title: Font Formatting - Type Size
tags: []
id: '4013'
categories:
  - - Misc
comments: false
date: 2018-10-04 15:57:18
---

[![](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)When using a word processor/text editor, you will often change the size of your type, for various reasons by setting a numeric size option. You can achieve the same results when creating an R Markdown document using some inline LaTeX code. I've written before about a setting the font size in your document by using settings within your YAML header, but noted that you could only use one of three options (10pt, 11pt or 12pt) in PDF output. This limitation is due to standard latex classes only accepting those three. To overcome this, you can use inline LaTeX code in your R Markdown document to specify relative sizes for your text. Instead of using 12pt or 14 pt, you use Huge or Large or other variations to produce different sizes of text. Here is the output from an example R Markdown document that shows the different sizes:
<!-- more -->
[![](http://edpflager.com/wp-content/uploads/2018/10/Font-Sizes-300x210.png)](http://edpflager.com/wp-content/uploads/2018/10/Font-Sizes.png) To achieve this results, use the various codes in the below R Markdown document, using and then the descriptor for the text size you want.

\---
title: "Font test"
output:
   pdf\_document:
      latex\_engine: xelatex
mainfont: Calibri Light
monofont: Arial
---


\`\`\`{r setup, include=FALSE}
knitr::opts\_chunk$set(echo = TRUE)
\`\`\`

## Font size information comes from https://en.wikibooks.org/wiki/LaTeX/Fonts

Huge This is HUGE

huge This is not as huge

LARGE This is really LARGE

Large This is somewhat Large

large This is large

normalsize (default)

small This is a small font

footnotesize This is footnotesize

scriptsize This is scriptsize (sub or super)

tiny This is just tiny!

That's it!