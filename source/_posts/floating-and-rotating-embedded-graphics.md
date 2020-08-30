---
title: Floating and Rotating Embedded Graphics
tags:
  - How-to
  - howto
  - R Markdown
  - technical
id: '4046'
categories:
  - - Blog
  - - Misc
  - - R
  - - R Markdown Cookbook
comments: false
date: 2018-10-12 12:29:09
---

[![](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png) The combination of R Markdown and Knitr provides a very versatile environment for generating output. You can mix using Pandoc and LaTeX packages in your document to achieve a remarkable level of control over the output you produce. Previously I covered some simple methods to embed graphics in your R Markdown file output using just Pandoc syntax. The results were passable. We could include a caption, resize the image in a couple of ways, and include our image inline so that it it appeared within a row of text. What we couldn't do very well was position the image in the center of our PDF document output. This post will cover using LaTeX packages to achieve that along with a couple of other ways we can manipulate our images.
<!-- more -->
To include LaTeX functions in our R Markdown document, we need to specify them in the YAML header of our file. For this example, I am using the graphix and float packages:

header-includes: usepackage{graphicx} 
                 usepackage{float} 

To embed a full size copy of the image, we just call it like below, and it generates an image as we would expect. Here is the code and a resulting screen shot:

includegraphics{pathtoimage}

There is one thing to take into account here. If there is text proceeding the image, include a blank line between the text and the image code to ensure the text stays above the image. [![](http://edpflager.com/wp-content/uploads/2018/10/includegraphix-300x262.png)](http://edpflager.com/wp-content/uploads/2018/10/includegraphix.png) You can also manage the size of the image here in a different way then I posted about previously with the pandoc options. The **includegraphics** function uses **textwidth** to reference the length of a line of text in your document. If you want to have your graphic image be dynamic depending on the length of the line, you can include code to calculate its width as a percentage of the **textwidth**. For example, to make it a quarter of the width of a line of text, you would specify the image embedding this way, being sure to use brackets around the size calculation and braces around the image name:

includegraphics\[width=0.25textwidth\]{gimp.png}

You can also specify absolute dimensions for your image using cm, mm or in values, like this:

includegraphics\[height=3in\]{gimp.png}

Just as with the Pandoc syntax, when generating a PDF, whichever value you set as the smallest is the one that **includegraphics** uses with the aspect ratio of the image maintained.

##### ROTATE AN IMAGE

Rotating an image using the LaTeX function **includegraphics** involves two settings. The first is an angle to determine how much you wish to rotate the image. Values range from 0 to 360, the same as normal circle coordinates. Second, define an origin value to set the point round which the image is rotated. Think of it as a pin in your image that keeps that part of the image stationary, while the rest of the image rotates around it. Options for the origin value are not granular, but define general areas. C would denote the center, t is top, and b is bottom. L and R define left and right. You can combine them into logical pairings as well, such as lc (left center), rb (right bottom) or tc (top center). Depending on the dimensions of your original image, using different values for your origin point may not result in much appreciable different.  In this example, using this code, produces the resulting image that follows it:

includegraphics\[height=1in,angle=155,origin=lc\]{gimp.png}

[![](http://edpflager.com/wp-content/uploads/2018/10/rotate155-2-e1539371479559.png)](http://edpflager.com/wp-content/uploads/2018/10/rotate155-2.png)

##### GRAPHICS SEQUENCE

Finally, if there are multiple commands you would like to include for your image, define an image sequence, followed by the commands. The sequence starts with a begin statement with figure in braces. Follow that with brackets around the option \[htbp!\]. This tells your LaTeX interpreter the preferred placement order of your image: h=here, t=top of current page, b=bottom of the current page, and p=top of the next page. All of these are dependent on how much space is still available on the current page, and how the size of the image. The final gives LaTeX some leaway in how exact it in in following the placement options. Leave the ! out to have it be strict in its interpretation. After the opening line of the sequence, include the various processing commands. Here is an example, with an added centering to tell LaTeX to center the image, and a caption{GIMP logo} command to add a caption, which will include a sequence indicator:

begin{figure}\[htbp!\]
centering
includegraphics\[width=0.25textwidth, angle=75, origin=lc\]{gimp.png}
caption{GIMP logo}
end{figure}

Finally close the sequence with the end{figure}  command. The resulting image looks like this in my sample document: [![](http://edpflager.com/wp-content/uploads/2018/10/rotatecaption-e1539372334593.png)](http://edpflager.com/wp-content/uploads/2018/10/rotatecaption.png) And here is the full text of the R Markdown document:

\---
title: "GraphicsFloat"
output: pdf\_document
header-includes: usepackage{graphicx}
                 usepackage{float}
                 usepackage{blindtext}
---

\`\`\`{r setup, include=FALSE}
knitr::opts\_chunk$set(echo = TRUE)
\`\`\`
blindtext

begin{figure}\[htbp!\]
centering
includegraphics\[width=0.25textwidth, angle=75, origin=lc\]{gimp.png}
caption{GIMP logo}
end{figure}

blindtext