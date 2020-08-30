---
title: Embedding Graphics
tags:
  - cookbook
  - How-to
  - R Markdown
id: '4044'
categories:
  - - Blog
  - - Misc
  - - R
  - - R Markdown Cookbook
comments: false
date: 2018-10-09 15:40:02
---

[![](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)The English idiom or cliche, that "a picture is worth a thousand words" does convey some truth. People by and large tend to be visual. We internalize concepts and ideas more easily that can be represented by visuals than be text alone.  So when working with R Markdown documents, you may want to embed graphical elements into your document. In this post, I will cover some of the basics of embedding graphics when generating PDF documents using R Markdown. I have several more posts queued up on this topic that will cover some more in-depth concepts, but for now let's look at some simple methods.
<!-- more -->
To insert an image in an R Markdown file that produces a PDF, Word document or an HTML file, you can use the PANDOC syntax (be sure to include the exclamation point):

!\[\](/path/to/image.png)

The brackets can be used to include a caption for your image if you want, or you can leave them empty for no caption. This command also works inline, but keep in mind that it will produce a lot of whitespace around your graphic. In these screen shots I am using the [GIMP](http://www.gimp.org) logo created by using the logo brush in that application. [![](http://edpflager.com/wp-content/uploads/2018/10/pandoc-graphics-e1539110259290.png)](http://edpflager.com/wp-content/uploads/2018/10/pandoc-graphics.png) The pandoc command does give you some options to control the size of your image. Just include the option in braces after the image file location:

!\[\](/path/to/image.png){width=25%}

This will scale your image to 1/4 of its original size, which is what I did in the screen shot above. You can also use absolute values for your width or height, by specifying them. When using R Markdown, however, I have found that the smaller of the two values will determine the size of the graphic in the final document, with the aspect ration being maintained. For example, embedding your image like this:

!\[\](gimp.png){width=300px height 50px}

won't produce a skewed image in a PDF or HTML, but instead will produce a very small image. Including the slash at the end of the line will help anchor the image so it does not end up floating near the bottom of the page. The output document in Word format will be skewed however. A comparison of the two is below: [![](http://edpflager.com/wp-content/uploads/2018/10/pandoc2.png)](http://edpflager.com/wp-content/uploads/2018/10/pandoc2.png)[![](http://edpflager.com/wp-content/uploads/2018/10/wordskewed.png)](http://edpflager.com/wp-content/uploads/2018/10/wordskewed.png) Coming up I'll cover more embedding graphics in your R Markdown documents with control over centering your images, and wrapping text around them.