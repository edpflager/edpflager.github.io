---
title: Font Formatting - Coloring and Emphasis
tags:
  - cookbook
  - guides
  - How-to
  - howto
  - R Markdown
  - technical
id: '4002'
categories:
  - - Blog
  - - Business Intelligence
  - - Misc
  - - R
  - - R Markdown Cookbook
comments: false
date: 2018-09-28 11:51:08
---

[![](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown.png)](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown.png)When working on an R Markdown document that will be output to a PDF, you may want to add a bit of color or embellishment to emphasize important information. By using the LaTeX function **xcolor** in your R Markdown code, you apply a wide palette of colors to enhance your text. The documentation for Xcolor is [available online](http://textdoc.net/pkg/xcolor) and includes a considerable amount of information on using the function in a LaTeX environment. Its not all applicable to the R Markdown library but I have been able to get the following color options to work when generating PDF output. Unfortunately it does not work in HTML or Word output. As with all enhancements, you should use this option sparingly. Too much of a good thing is likely to turn your readers off, if you are sharing the output. Also keep in mind those with visual impairments who can't discern different colors as well.
<!-- more -->
First we need to define our YAML header in the R Markdown file to include the LaTeX package **xcolor**. The header-includes option handles this. And be sure to watch your indentations.

\---
title: "Font Colors and Emphasizing"
output:
    pdf\_document:
      latex\_engine: xelatex
mainfont: Calibri Light
monofont: Arial
fontsize: 12 pt
header-includes: usepackage{xcolor}
---

#### COLOR

Once you have that option in your file, to specify a color for your font, proceed your text with the inline textcolor function, and then enclose your text in braces {} like this:

textcolor{colorname}{This is printed in your colorname}

The options I have tested successfully include:

*   lightgray
*   gray
*   darkgray
*   lime
*   green
*   olive
*   teal
*   blue
*   cyan
*   brown
*   magenta
*   red
*   violet
*   orange
*   purple
*   pink
*   yellow
*   white

Black is the default color when you don't use the textcolor function. My sample code for testing this function looks like this:

\---
title: "Font Colors and Emphasizing"
output:
    pdf\_document: 
       latex\_engine: xelatex
mainfont: Calibri Light
monofont: Arial
fontsize: 12 pt
header-includes: usepackage{xcolor}
---

\`\`\`{r setup, include=FALSE}
knitr::opts\_chunk$set(echo = TRUE)
\`\`\`
By using the LaTeX function textcolor you can add some color to your R Markdown PDF documents. 

textcolor{lightgray}{This is lightgray.}

textcolor{gray}{This is gray.}

textcolor{darkgray}{This is darkgray.}

textcolor{lime}{This is lime.}

textcolor{green}{This is green.}

textcolor{olive}{This is olive.}

textcolor{teal}{This is teal.}

textcolor{blue}{This is blue.}

textcolor{cyan}{This is cyan.}

textcolor{brown}{This is brown.}

textcolor{magenta}{This is magenta.}

textcolor{red}{This is red.}

textcolor{violet}{This is violet.}

textcolor{orange}{This is orange.}

textcolor{purple}{This is purple.}

textcolor{pink}{This is pink.}

textcolor{yellow}{This is yellow.}

textcolor{white}{This is white.} The proceeding text is white. .

This results in a PDF document that looks like this: [![](http://edpflager.com/wp-content/uploads/2018/09/Font-Colors-1024x1011.png)](http://edpflager.com/wp-content/uploads/2018/09/Font-Colors.png)

#### EMPHASIS

If you don't want to use color in your document, you can also emphasize important portions of your document using the traditional methods: bold, underline and italicizing your text. These are accomplished using three different inline functions as shown below:

textbf{This is bold} 

textit{This is italicized} 

underline{This is underlined} 

Dropping those lines into our R Markdown document produces this output: [![](http://edpflager.com/wp-content/uploads/2018/09/Emphasis.png)](http://edpflager.com/wp-content/uploads/2018/09/Emphasis.png)

#### COMBINING THE TWO

Finally, if you'd like to combine the options, its just a matter of nesting the commands in the right order. From my experience, the color setting should be first, followed by the emphasis settings. As an example, to include red bold text, you would enter the command like this: textcolor{red}{textbf{And you can combine them by nesting}} Which produces this PDF: [![](http://edpflager.com/wp-content/uploads/2018/09/nested-color-and-emphasis.png)](http://edpflager.com/wp-content/uploads/2018/09/nested-color-and-emphasis.png)