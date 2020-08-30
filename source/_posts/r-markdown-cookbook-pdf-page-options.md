---
title: PDF Page options
tags:
  - cookbook
  - guides
  - How-to
  - howto
  - R Markdown
  - technical
id: '3976'
categories:
  - - Blog
  - - Business Intelligence
  - - Misc
  - - R
  - - R Markdown Cookbook
comments: false
date: 2018-09-25 14:50:21
---

[![](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)This time around I'm deep diving into tips for formatting PDF output with R Markdown. One advantage I have found with PDFs over EPUB files is that when you create a PDF it looks the way you intended to, almost a snapshot of a printed page. While EPUB has some advantages over PDFs (including much smaller file sizes), a PDF to me is still preferable for technical documentation where there are screen shots, figures and charts. When generating a PDF with R Markdown there are a few options you can define to determine how your pages look, including the document size (letter, legal, A4, etc), page orientation, and even the set different margins for each side of the page. All of these options are specified within the YAML metadata header portion of your R Markdown document, and make use of many LaTex functions. By default, R Markdown uses the pdflatex engine, but if you plan to use fonts from your system, or need support for Unicode, you should use one of the alternatives: xelatex or lualatex.
<!-- more -->
First lets look at how to set up your header section to include LaTex support. You define your document title, then specify your output format. In this case a pdf\_document. If you are going to be printing tables of data, [include the kable package](http://edpflager.com/2018/09/14/r-markdown-cookbook-table-formatting-for-pdf/) for better formatting control and then specify the latex\_engine to use. The result should resemble this:

title: "PDFTest"
  output: 
    pdf\_document:
      df\_print: kable
      latex\_engine: xelatex

In a previous post I covered [how to use system fonts](http://edpflager.com/2018/09/12/r-markdown-cookbook-font-formatting/) in your PDF output from R Markdown, so I will not cover that here.

#### PAPER SIZE

Starting with the dimensions of our document, we can define the size of our PDF output "page" using the papersize option. R Markdown and LaText support both the ISO 216 standard and the North American standard, so you have numerous options to use, although the default is the North American letter size. The basic format for the papersize option is:

papersize: size  -- such as letter/legal/ledger/tabloid/a4/b3

[BelightSoft has a good article](https://www.belightsoft.com/products/resources/paper-sizes-and-formats-explained) on the various paper sizes used throughout the world.

#### PAGE ORIENTATION

[![](http://edpflager.com/wp-content/uploads/2018/09/portrait-landscape-300x213.jpg)](http://edpflager.com/wp-content/uploads/2018/09/portrait-landscape.jpg)Besides the size of the output "page", you can also set the orientation of the page. Typically page orientation is set to portrait as the default. Portrait mode means the width of the page is shorter than the length. However, if you want to switch it to make your output print horizontally across the wider portion of the documentation  you would use landscape orientation. To use portrait mode, you don't need to add anything to your YAML header, although you can specify portrait as an option if you want it to be explicit. If you'd prefer to set your documentation to landscape, add this line to your header section:

classoption:
   landscape

#### MARGINS

Our final page setup option for R Markdown is margins for your PDF document. Generally margins are consistent on all four sides of a document page, with a default of 1 inch in the North America system or 2.5 centimeters in the ISO 216 standard. To override those defaults, you can use the geometry option in your YAML header, and specify your margins. If you want to have the same margin on all four sides of the document, use the margin option, like this:

geometry:
    margin= .75in

If you'd like to specify different margin widths on various sides, you can specify them individually or use a default for all of them, and then override the default for a specific side. As an example, to have a 2.5cm margin on the top, bottom and right, with a 5cm margin on the left, enter this in your YAML header :

geometry:
    margin= 2.5cm,
    left=5cm

You can also specify each side individually using several different options, seperated by commas, followed by your preferred measurement:

*   left | lmargin | inner
*   right | rmargin | outer
*   top | tmargin
*   bottom | bmargin

To wrapup, here is the YAML header using the options discussed above: [![](http://edpflager.com/wp-content/uploads/2018/09/YAML-example-263x300.png)](http://edpflager.com/wp-content/uploads/2018/09/YAML-example.png)   The R logo is © 2016 [The R Foundation](https://www.r-project.org/logo/). Used under terms of the CC-BY-SA 4.0 license.