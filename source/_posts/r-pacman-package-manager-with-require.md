---
title: R - Pacman package manager with !require
tags:
  - howto
id: '4154'
categories:
  - - Blog
  - - R
comments: false
date: 2018-11-11 18:09:40
---

[![](http://edpflager.com/wp-content/uploads/2018/09/Rlogo-300x232.png)](http://edpflager.com/wp-content/uploads/2018/09/Rlogo.png)A few weeks back, I wrote a [quick post](http://edpflager.com/2018/10/26/r-quick-code-to-install-needed-packages/) about a one-line piece of R code that will check for required libraries, and if they are not installed will attempt to install them. Nice tip and helpful to use. This past week, I attended an R Users Group meeting in Ann Arbor, MI where Sam Firke was presenting about his package [Janitor](https://github.com/sfirke/janitor). When he showed some code, he mentioned a package he uses called [pacman](https://cran.r-project.org/web/packages/pacman/index.html).  Its a utility type package, designed to manage R packages, hence the name pacman. From the description on the CRAN site, it sounds dry, and not too interesting, but for locating information about your R installation, it is extremely helpful.  But one pacman function that Sam was using in his code caught my attention: p\_load. From the package documentation:
<!-- more -->
\[p\_load\] checks to see if a package is installed, if not it attempts
to install the package from CRAN and/or any other repository in the
pacman repository list.

The other feature included in this function is the ability to use a vector of packages as part of the call. So rather than have multiple lines to load several packages using my previous tip, like this:

if( !require(janitor){ install.packages('janitor')}
library(janitor)

if( !require(tidyverse)){ install.packages('tidyverse')}
library(tidyverse)

if( !require(odbc)){ install.packages('odbc')}
library(odbc)

You could instead use this:

if( !require(pacman)){ install.packages('pacman')}
library(pacman) \# for loading packages

p\_load(janitor, tidyverse, odbc)

The first two lines are a repeat of my earlier tip to check to see if pacman is installed. If its not, then install it. Then using the pacman p\_load function, load the required packages for the R script being run. By default, packages are installed if they are not present when you try to load them with p\_load! If you are trying to use packages from GitHub instead of the CRAN repository, you can use p\_load\_gh instead. The rest of the functionality is the same, although with github repositories you do need to enclose each repository/package in quotes.  So for example to load the lvplot package from Hadley Wickham's repository, you would use:

p\_load\_gh("hadley/lvplot")

That's it!