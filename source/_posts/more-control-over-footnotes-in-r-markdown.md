---
title: More Control over Footnotes in R Markdown
tags:
  - howto
  - R Markdown
  - technical
id: '4319'
categories:
  - - Blog
  - - R
  - - R Markdown Cookbook
comments: false
date: 2019-01-21 19:17:03
---

I[![](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)n R Markdown include footnotes in documents to provide a reference or comment that may provide additional context or an aside to the topic. Traditionally, these numbered notes are placed at the bottom of the page and a corresponding superscript number is added to the text body at the relevant text. R Markdown supports footnotes natively without the need for any additional packages.I'll cover that first, and then show how to gain more control over footnotes using an additional LaTeX package. The native footnote syntax is simple, and requires just a paired set of footnote tags. **\[^1\]** - denotes where the footnote superscript notation should be placed. **\[^1\]: {footnote text}** - is the corresponding footnote text. The character within the brackets has to be match the superscript tag and be followed by the trailing the colon. This footnote text can be placed at any spot in the document, but for clarity's sake, place it close to the text it refers to.
<!-- more -->
To illustrate more clearly, here is an example RMarkdown document: [![](http://edpflager.com/wp-content/uploads/2019/01/FootnoteNormalCode-1024x274.png)](http://edpflager.com/wp-content/uploads/2019/01/FootnoteNormalCode.png) And the rendered document from RMarkdown: [![](http://edpflager.com/wp-content/uploads/2019/01/FootnoteNormal-1024x360.png)](http://edpflager.com/wp-content/uploads/2019/01/FootnoteNormal.png) R Markdown and Knitr will generate appropriate footnote numbers in sequence according to the placement of the opening footnote tags regardless of the tags being used. Character notation works as well as numbers.

##### USE LATEX PACKAGE FOOTNOTE FOR MORE CONTROL

Use the LaTeX package [**FOOTNOTE**](https://ctan.org/pkg/footnote) from Mark Wood­ing by adding this line to the YAML section of the R Markdown document:

 usepackage{footnote}

The footnote package provides features to generate footnotes using roman numerals (lower and upper case), alphabetic characters (lower and upper case) or even the sequence of nine keyboard symbols pictured here. [![](http://edpflager.com/wp-content/uploads/2019/01/FootnoteSymbols-150x150.png)](http://edpflager.com/wp-content/uploads/2019/01/FootnoteSymbols.png)Be careful using the symbols because more than nine footnotes will generate an error message. To select the footnote option to use, at the beginning of the R Markdown document, or at least before the first footnote, add the appropriate command from this list:

renewcommand{thefootnote}{fnsymbol{footnote}} - symbols in the picture above
renewcommand{thefootnote}{arabic{footnote}} - the standard footnote pattern: 1,2,3,etc
renewcommand{thefootnote}{Roman{footnote}} - upper case Roman numerals (I,II, III, IV, etc)
renewcommand{thefootnote}{roman{footnote}} - lower case Roman numerals (i,ii,iii,iv, etc)
renewcommand{thefootnote}{Alph{footnote}} - upper case characters (A,B,C,etc)
renewcommand{thefootnote}{alph{footnote}} - lower case characters (a,b,c, etc)

Turning off the superscript for footnotes completely can be accomplished by including this command at the beginning of the document:

letthefootnoterelaxfootnote

##### LATEX PACKAGE FOOTMISC PROVIDES MORE OPTIONS

Originally created by Robin Fair­bairns, the [**FOOTMISC**](https://ctan.org/pkg/footmisc) package in LaTeX modifies the standard footnote options, and adds some new features. Some of the more interesting ones are highlighted below, but check out the documentation at the link above for more. Suppress the printing of the traditional horizontal line at the bottom of the page before footnotes can be turned on by inserting the following near the top of the R Markdown document:

renewcommandfootnoterule{}

Another interesting option in **FOOTMISC** is the inclusion of several new symbol sets for footnote notation. To use one of the predefined symbol sets (brighthurst, chicago, or wiley) that removes the duplicate entries from the original set and adds new ones, use these commands:

setfnsymbol{symbol set name, e.g. chicago}
renewcommand{thefootnote}{fnsymbol{footnote}}

In LaTeX, the package allows the definition of new sets, although I have not been able to get it to work in R Markdown. Keep in mind that the formatting options chosen for the footnote will remain consistent throughout the document. There are also additional packages available in LaTeX to provide more control over footnote formatting, including: footbib and footmisx. Check the CTAN.ORG website for more information.