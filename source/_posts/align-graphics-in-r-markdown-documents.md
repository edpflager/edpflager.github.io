---
title: Align Graphics in R Markdown Documents
tags:
  - howto
  - R Markdown
  - technical
id: '4115'
categories:
  - - Blog
  - - Misc
  - - R
  - - R Markdown Cookbook
comments: false
date: 2018-10-31 03:46:18
---

[![](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)This time around I am covering aligning graphics in PDF putout for R Markdown. Unfortunately this tip only works in PDF output, not in HTML or Word. Occasionally, perhaps rarely for some, you may want to place two graphics next to each other in your R Markdown documents. By default you can have the bottom edge of your graphics (aka the baseline) align easily, using the [technique](http://edpflager.com/2018/10/09/embedding-graphics/) I've covered previously:

!\[\](3inchSquare.png) !\[\](1inchSquare.png)

The output that is produced does have a gap between the images though, because you need a separation in the code between the first image and the second one so the KNITR engine doesn't get confused.
<!-- more -->
To make the images flush against each other, we can use the LaTeX function graphicx by specifying it in the YAML header. Then we call our images in one line of code:

includegraphics\[\]{images/3inchsquare.png}includegraphics\[\]{images/1inchsquare.png}

Here is a comparison of the resulting output: [![](http://edpflager.com/wp-content/uploads/2018/10/AlignImages-206x300.png)](http://edpflager.com/wp-content/uploads/2018/10/AlignImages.png) By using the raisebox command that is part of the graphicx function, we get additional control over our image placement. The syntax for the raisebox command is simple: **raisebox{height to raise image}** and follow it with an includegraphics command enclosed in braces. For example if we want to have the images aligned on the center of each image, we can use a calculated height formula, followed by the image, and then a second raisebox command, followed by a height calculation. So for our two boxes, the command would be:

raisebox{-0.5height}{includegraphics\[\]{images/3inchsquare.png}}raisebox{-0.5height}{includegraphics\[\]{images/1inchsquare.png}}

Which results in this placement of the images: [![](http://edpflager.com/wp-content/uploads/2018/10/AlignCenter-150x150.png)](http://edpflager.com/wp-content/uploads/2018/10/AlignCenter.png) You can also align your images along the topline of the images, using this command:

raisebox{-height}{includegraphics{images/3inchsquare.png}}raisebox{-height}{includegraphics{images/1inchsquare.png}}

The resulting placement in your PDF document output would look like this: [![](http://edpflager.com/wp-content/uploads/2018/10/AlignTop-150x150.png)](http://edpflager.com/wp-content/uploads/2018/10/AlignTop.png) There are a number of other placements that are possible using **raisebox**, and I have illustrated some of them in the code for my example R Markdown document below.

\---
title: "Aligning Graphics"
output:
   pdf\_document
header-includes: usepackage{graphicx} 

---

\`\`\`{r setup, include=FALSE}
knitr::opts\_chunk$set(echo = TRUE)
\`\`\`
Default placement

!\[\](3inchSquare.png) !\[\](1inchSquare.png)

\*\*\*

includegraphics\[\]{images/3inchsquare.png}includegraphics\[\]{images/1inchsquare.png}


\*\*\*

Puts the baseline at midway of the first image. The second images bottom will be placed there.

raisebox{-0.5height}{includegraphics\[height=3in\]{images/3inchsquare.png}}{includegraphics\[\]{images/1inchsquare.png}}

\*\*\*

Puts the baseline at a specific point of the first image. The second image's bottom will be placed there. Use a negative value! 

raisebox{-1in}{includegraphics\[height=3in\]{images/3inchsquare.png}}{includegraphics\[\]{images/1inchsquare.png}}

\*\*\*

If you use a positive value, the second image's bottom will be placed at that point below the first image's bottom edge. 

raisebox{1.5in}{includegraphics\[height=3in\]{images/3inchsquare.png}}{includegraphics\[\]{images/1inchsquare.png}}

\*\*\*

raisebox{-0.5height}{includegraphics\[\]{images/3inchsquare.png}}raisebox{-0.5height}{includegraphics\[\]{images/1inchsquare.png}}

\*\*\* 

Top align both figures

raisebox{-height}{includegraphics{images/3inchsquare.png}}raisebox{-height}{includegraphics{images/1inchsquare.png}}