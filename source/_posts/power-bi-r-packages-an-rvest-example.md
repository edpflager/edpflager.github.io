---
title: Power BI R Packages - An RVEST example
tags:
  - howto
  - rvest
  - technical
id: '4212'
categories:
  - - Business Intelligence
  - - Misc
  - - R
comments: false
date: 2018-12-04 18:29:19
---

[![](http://edpflager.com/wp-content/uploads/2018/12/rvest-copy-259x300.png)](http://edpflager.com/wp-content/uploads/2018/12/rvest-copy.png)Activity in my day job often provides inspiration for content here, and this post is an example of that.  In my work, I use Microsoft's SQL Server and Power BI. Both applications now offer some level of support for R:

*   Microsoft now includes an R server with SQL Server
*   Power BI allows you to use R visualizations to provide functionality that is not included natively.
*   If you have an R script that cleans and preps your data, Power BI use it as a data source for visualizations and analytics.

Power BI doesn't support all R packages. Of the 10,000 plus packages available in the CRAN repository only 850 or so packages are currently supported. This [webpage](https://docs.microsoft.com/en-us/power-bi/service-r-packages-support) includes an explanation for what they do support and why. All of that is a preface to this post. At the above link is a table of the supported Power BI packages with version number and a URL for the package on CRAN. I wanted to capture that information for incorporation into some (non-work) documentation. I could easily copy and paste it into a spreadsheet or a text file, but I wanted to set it up as a repeatable process in case the website is updated. I could then capture the updated information quickly and easily. Enter [RVEST](https://cran.r-project.org/web/packages/rvest/index.html) - a screen scrapping package for R.
<!-- more -->
RVest is one of many R packages authored by Hadley Wickham, famous for GGPLOT2 and the wider Tidyverse set of packages. Its purpose is to easily harvest or scrape webpages. To get the data from the Microsoft webpage about supported R packages, we can use the RVest package to do about 90% of the work and with a little help from Tibble, we can shape the data in to usable format as a data frame. I will present this process first in a long format showing each step that calls different RVest functions, and at the end, I will wrap it up with the Magrittr pipe operator to make the code a bit cleaner. Before we start we need to use some kind of tool to find the element name on the page that we want to harvest. Hadley Wickham on the GitHub page for RVest recommends SelectorGadget an extension for Google Chrome. In RStudio, at a command line enter the following to get more information:

vignette("selectorgadget")

Using SelectorGadget on the Microsoft webpage, we see that the table we are interested in is not named, but it is the first one on the page. That is enough information to get started.

#### INITIAL VERSION

Switching over to R Studio after loading our needed packages (rvest and tibble) we call the read\_html function and pass it the webpage address we want to harvest.

\# URL to scrape
page <- read\_html("https://docs.microsoft.com/en-us/power-bi/service-r-packages-support")

Using the harvested webpage information stored in our page list, we want to retrieve the table elements sections.

 # Element type to grab from page
nodes <- html\_nodes(page,"table")

Since there are multiple tables on that webpage, and the individual tables are not given unique names we specify the table number as the argument for the html\_table function and pass it to a new variable. In this case its the first table:

\# we want the first table
msrpackages <- html\_table(nodes\[1\])

Finally, we pass the table variable (msrpackages) to the as.data.frame function, nested within an as\_tibble function, and store the data frame in a new variable called RPackages.

RPackages <- as\_tibble(as.data.frame(msrpackages))

To see the data stored in the tibble, we call the View function with the RPackages element as the argument to generate the results are below.

View(RPackages)

[![](http://edpflager.com/wp-content/uploads/2018/12/MSRPackagesView-300x143.png)](http://edpflager.com/wp-content/uploads/2018/12/MSRPackagesView.png) The long code for this script then looks like below (see my previous post about [pacman](http://edpflager.com/2018/11/12/r-pacman-package-manager-with-require/) for an explanation of that function):

\# clear environment
rm(list=ls())

if( !require(pacman)) { install.packages('pacman')}
library(pacman)

p\_load(rvest, tibble)

# URL to scrape
page <- read\_html("https://docs.microsoft.com/en-us/power-bi/service-r-packages-support")

# Element type to grab from page
nodes <- html\_nodes(page,"table")

# Since there are multiple tables on that webpage, we need to specify
# that we want the first table
msrpackages <- html\_table(nodes\[1\])

# Convert the list into a tibble/data frame.
RPackages <- as\_tibble(as.data.frame(msrpackages))

# Display the data
View(RPackages)

#### CLEANER VERSION

The code is fairly straightforward, but it can be cleaner if we incorporate the Magrittr pipe function, which Hadley Wickham [indicates](https://github.com/hadley/rvest) that rvest was designed to do.

\# clear environment
rm(list=ls())

# Load pacman to make sure all other packages are available
if( !require(pacman)) { install.packages('pacman')}
library(pacman)

# Pacman function to load needed packages
p\_load(rvest, tibble, magrittr)

# URL to scrape
page <- read\_html("https://docs.microsoft.com/en-us/power-bi/service-r-packages-support")

# Perform rvest tasks to get data into a usable format
RPackages <- page %>%
html\_nodes("table") %>%
.\[1\] %>%
html\_table %>%
as.data.frame() %>%
as\_tibble

# Display the data
View(RPackages)

So there you have it - a simple script to harvest the table from Microsoft's webpage showing Power BI support for CRAN packages.