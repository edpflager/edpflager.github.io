---
title: Tidyverse installation on Linux Mint
tags:
  - howto
  - Mint
  - technical
  - tidyverse
id: '4342'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
  - - R
comments: false
date: 2019-01-24 15:28:44
---

If you've been using R for even a sh[![](http://edpflager.com/wp-content/uploads/2019/01/tidyverse-139x150.png)](http://edpflager.com/wp-content/uploads/2019/01/tidyverse.png)ort time, you more than likely have heard of the Tidyverse from RStudio and originally developed by Hadley Wickham.  As explained on the [tidyverse website](https://www.tidyverse.org/) this set of packages works together with a common data representation methodology and API design.  They provide a host of functionality to clean and tidy your data to make it easier to analyze. Similar data representation between packages lets you define an entity for one operation, and use that entity through an entire workflow rather than creating intermittent entities along the way, resulting in more efficient code, and requiring fewer  system resources. The tidyverse methodology mimics good database design, with one row of data consisting of a single record or observation with variables assigned to columns in that row, and each value being assigned to its own cell. Having worked with databases for over a decade, the structure of tidy data makes sense. See Hadley Wickham's book [for more information](https://r4ds.had.co.nz/tidy-data.html). Currently there are 26 separate packages in the tidyverse, and even more that inter operate with them, so they can be a big part of your R workflow. The installation of the tidyverse is fairly easy in most situations, using the standard method  in R Studio or at the R command line:

install.packages("tidyverse")

But when setting up a new Linux Mint 19.1 system, that didn't work for me. The output included a long list of errors, and some helpful text to install this or that Debian package, buried in the text that was scrolling by. To make this process easier for Mint users, here are instructions on operating system requirements to install before attempting to install  the Tidyverse.
<!-- more -->
First make sure your apt-cache is up to date, then install the various packages I have strung together below.

 sudo apt-get update

 sudo apt-get install r-base-dev xml2 libxml2-dev libssl-dev libcurl4-openssl-dev unixodbc-dev 

Once you have installed the Mint additions, start R-Studio and then run the standard command for installing R packages:

    install.packages("tidyverse")

In a few minutes it should indicate success. Try it out with this code and you should see results like the image below:

    library(tidyverse)

    tidyverse\_packages

[![](http://edpflager.com/wp-content/uploads/2019/01/tidyverse-screen.png)](http://edpflager.com/wp-content/uploads/2019/01/tidyverse-screen.png)