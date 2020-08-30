---
title: Lorem Ipsum text
tags:
  - cookbook
  - How-to
  - howto
  - R Markdown
  - technical
id: '3993'
categories:
  - - Blog
  - - Business Intelligence
  - - Misc
  - - R
  - - R Markdown Cookbook
comments: false
date: 2018-09-27 11:35:05
---

[![](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown.png)](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown.png)A quick tip this time out for R Markdown. For many reasons you may need to add blocks of text to your R Markdown documents. From [Infogalactic.com](https://infogalactic.com/info/Lorem_ipsum), "in publishing and graphic design ... filler text is commonly used to demonstrate the graphic elements of a document or visual presentation. Replacing meaningful content with placeholder text allows viewers to focus on graphic aspects such as font, typography, and page layout without being distracted by the content. It also reduces the need for the designer to come up with meaningful text, as they can instead use quickly-generated lorem ipsum." If you need to generate filler text for you R Markdown document, you can do so very quickly.
<!-- more -->
In the XAML header section of your file, add this:

header-includes:
    usepackage{blindtext}

Then in the body of your document locate where you want your filler text to be. The command you enter in your document will depend on the amount of filler text you want to generate. For a short section of filler text use the command:

blindtext

For a longer section of filler text, use:

Blindtext

The only difference between the two is a capital or lowercase B. The shell code for the R Markdown file would them look like this: [![](http://edpflager.com/wp-content/uploads/2018/09/Lorem-Ipsum-300x211.png)](http://edpflager.com/wp-content/uploads/2018/09/Lorem-Ipsum.png) The resulting output is: [![](http://edpflager.com/wp-content/uploads/2018/09/Lorem-Ipsum-results-1024x890.png)](http://edpflager.com/wp-content/uploads/2018/09/Lorem-Ipsum-results.png)