---
title: Wrap text around Graphics
tags:
  - cookbook
  - guides
  - How-to
  - R Markdown
  - technical
id: '4048'
categories:
  - - Blog
  - - Misc
  - - R
  - - R Markdown Cookbook
comments: false
date: 2018-10-14 04:15:48
---

[![](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)Building on my earlier posts about including graphics in your R Markdown output, this post will demonstrate how to wrap text around graphics in your R Markdown document. The technique will produce output like this where text is wrapped around the figure at the left. Wrapping text can provide a varied appearance to your document and minimizes white space. It can aid in readability but also tends to deemphasize the importance of the image, so you this technique sparingly.
<!-- more -->
To wrap text around a graphical element, we need to include the wrapfig package as part of our YAML header. Including the packages specified previously for working with graphics in R Markdown documents, our header-includes section looks like this:

header-includes: usepackage{graphicx}
                 usepackage{float}
                 usepackage{wrapfig}
                 usepackage{blindtext}

(I am using the [blindtext option covered previously](http://edpflager.com/2018/09/27/r-markdown-cookbook-lorem-ipsum-text/) to produce some random text for this example). Within the document, we have inline code to define the start of the graphic wrapping sequence, and include several options. The sequence name must be wrapfigure. The options inclosed with brackets \[ \] are optional, while those within braces { } are required.

begin{wrapfigure}\[12\]{l}\[2pt\]{5cm}

1.  The first option \[12\] in this example, tells R Markdown how many lines to wrap around your graphic. Be careful of setting this value too low if your image is large, because subsequent lines may over-lay the image.
2.  The second option {l} above, indicates where to place the image. Allowed values are l or L for left, and r or R for right. In LaTeX, the upper case versions allow the image to float a bit, but in my experience I've seen no appreciable difference. LaTeX also allows i/I and o/O to place the image on the inside edge by the binding or the outside edge away from the binding. Again, I've not seen this work in R Markdown.
3.  For the third option, you can indicate how far you want the image to hang out into the margin. You can use an explicit value as I have done in the example line \[2pt\] or you can use a calculated value like \[0.25width\] to put a quarter of the graphic into the margin. If you use just \[width\] the entire graphic will be placed in the margin, and may be cut off if its a larger image. The wrapped text will be indented regardless of which option you use.
4.  Finally, the last option is used to indicate the width of the graphic element. This doesn't scale the image, but instead defined the "window" size around the graphic. If you specify a value that is too small here, the wrapped text may over-lay the image and if you specify a too large value, you may undo the wrapping you are trying to achieve.

After the beginning sequence line, you then include your graphic element definition, and optionally a caption element. The includegraphics line can use the same options that were covered in a previous post.

includegraphics\[width=0.25textwidth\]{gimp.png}
caption{GIMP logo}

Finally, you close the wrapfig sequence end{wrapfigure} Pulling all of this together, The R Markdown document looks like this:

\---
title: "GraphicsWrappingText"
output: pdf\_document
header-includes: usepackage{graphicx}
                 usepackage{wrapfig}
                 usepackage{blindtext}
---

\`\`\`{r setup, include=FALSE}
knitr::opts\_chunk$set(echo = TRUE)
\`\`\`

### Wrapfig
par
begin{wrapfigure}\[12\]{l}\[2pt\]{5cm}
includegraphics\[width=0.25textwidth\]{gimp.png}
caption{GIMP logo}
end{wrapfigure}
Blindtext

The end result is this, reproduced from above. [![](http://edpflager.com/wp-content/uploads/2018/10/WrapFig-1024x545.png)](http://edpflager.com/wp-content/uploads/2018/10/WrapFig.png)