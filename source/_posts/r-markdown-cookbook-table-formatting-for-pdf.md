---
title: Table formatting for PDF
tags:
  - cookbook
  - guides
  - How-to
  - howto
  - technical
id: '3884'
categories:
  - - Misc
  - - R
  - - R Markdown Cookbook
comments: false
date: 2018-09-13 19:40:52
---

[![](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)Using R Markdown to create PDF files for sharing, you can chose several different options for printing tables of data: **default, kable** and **tibble**. If you are using HTML, you have another option called **paged**, which allows you to specify the number of rows and columns to display, and the viewer can page between pages without having to scroll up and down a large table. I'll cover that option in a future post. To use the default style for PDF output, don't include a df\_print option in your R-Markdown YAML header section. To use kable or tibble add the df\_print option like below and follow it with the option you want to use.
<!-- more -->
Be sure to watch the indentation of the df\_print line.

\---
title: "Table example"
output:
   pdf\_document:
      df\_print: kable/tibble
---

Here are examples of the three outputs: [![](http://edpflager.com/wp-content/uploads/2018/09/dataframe-default-sample-300x253.png)](http://edpflager.com/wp-content/uploads/2018/09/dataframe-default-sample-e1536890311448.png) Fig 1 . Traditional table output [![](http://edpflager.com/wp-content/uploads/2018/09/dataframe-kable-sample-244x300.png)](http://edpflager.com/wp-content/uploads/2018/09/dataframe-kable-sample-e1536890265304.png) Fig 2. Kable table output [![](http://edpflager.com/wp-content/uploads/2018/09/dataframe-tibble-sample-297x300.png)](http://edpflager.com/wp-content/uploads/2018/09/dataframe-tibble-sample.png) Fig 3. Tibble table output If you do use **kable**, be aware that on wider tables the results may be cut off due to page sizes. To fix that, you will need to include the **kableExtra** and **magrittr** libraries. Then wrap the table function like this:

kable(head(bike)) %>% kable\_styling(latex\_options = c("striped","scale\_down"))

  Finally, although the latex\_option is called "scale\_down", what it does is make your table fit within the margins of your document. On tables that already fit comfortably, using this function will enlarge them to fit from left to right margin. Below is what the output looks like using **kable** for a wide data set, first using the default **kable** settings, and then using the **kable\_styling:** [![](http://edpflager.com/wp-content/uploads/2018/09/dataframe-kamble-wide-1024x408.png)](http://edpflager.com/wp-content/uploads/2018/09/dataframe-kamble-wide-e1536892335405.png) Fig 4 . Table without kable\_styling and with kable\_styling I included the option **striped** as well, which subtly highlights every other line for easier viewing.   The R logo is © 2016 [The R Foundation](https://www.r-project.org/logo/). Used under terms of the CC-BY-SA 4.0 license.