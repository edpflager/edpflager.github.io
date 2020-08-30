---
title: Install RStudio's Shiny Server on Linux Mint
tags:
  - cookbook
  - How-to
  - howto
  - install
  - Linux
  - Mint
  - technical
id: '4465'
categories:
  - - Blog
  - - Business Intelligence
  - - Misc
  - - R
comments: false
date: 2019-03-27 15:26:49
---

[![](http://edpflager.com/wp-content/uploads/2019/03/shiny.png)](http://edpflager.com/wp-content/uploads/2019/03/shiny.png)I'm back, after my day job related hiatus! This time I'm looking at installing RStudio's open source version of Shiny Server on Linux Mint. If you aren't familiar with Shiny, its a product from the [RStudio team](https://shiny.rstudio.com/) that provides a web framework to host interactive data analysis apps using a variety of different technologies including but not limited to CSS, HTMLWidgets and Javascript actions. You built your apps directly from the R programming language for data analytics and visualizations to embed R Markdown documents to present your analysis with notes, put together dashboards and develop other interactive data apps.

##### SYSTEM PREPARATION

My development environment is running Linux Mint 19.1 Cinnamon 64-bit with 4GB of RAM. When I started I removed most of the default applications to have just a basic GUI system and patched it to the most current versions of everything that remained. Then, I followed my previous post for [installing R and RStudio on Linux Mint](http://edpflager.com/2018/12/18/rstudio-on-linux-mint-19/) to get a basic R installation up and running.
<!-- more -->
I am assuming that if you are installing a server application, then you have root access via **sudo** to your machine. If you don't then this tutorial won't work for you. You are very likely to  get an error message like this:

Warning in install.packages :
  'lib = "/usr/local/lib/R/site-library"' is not writable

##### INSTALLATION

Open a terminal prompt and run RStudio as root:

sudo /usr/bin/rstudio

Shiny server runs under a user account called Shiny. When you install packages in R on Linux Mint by default, it installs those packages under **your** user account's R package library. But they need to be under the Shiny account's package library or preferably under the system-wide R package library so the Shiny account can access them. When RStudio comes up, in the console pane, install the Shiny package using the syntax below. The **repos** clause will force R to get the Shiny package from the RStudio Cran mirror and the **lib** clause will install it into the system library instead of your personal one so it is accessible to the Shiny account:

install.packages("shiny", repos = "https://cran.us.r-project.org", lib = "/usr/local/lib/R/site-library")

[![](http://edpflager.com/wp-content/uploads/2019/03/ShinyPackage.png)](http://edpflager.com/wp-content/uploads/2019/03/ShinyPackage.png)As part of the installation, a number of dependencies will be installed as well if they are not already present so it may take awhile to complete.

After the shiny installation completes, repeat the process with RMarkdown. This package is required for the default startup page for Shiny, and you'll probably be using it a lot with Shiny:

install.packages("rmarkdown", repos = "https://cran.us.r-project.org", lib = "/usr/local/lib/R/site-library")

Once completed, if you switch to the Packages tab in RStudio, you'll see a System Library header with Shiny and RMarkdown included along with a number of other packages. If you deploy any code to your server that uses packages that are not listed in the System Library, be sure to install them using the method outlined here.

BTW, if you'd prefer there is another way to do the package installation strictly via the command line. Because you are running this as **sudo** it will automatically push the packages in to the system library for R:

sudo su - 
-c "R -e "install.packages('shiny', repos='https://cran.rstudio.com/')""

Now exit R Studio and from a browser check the [RStudio Shiny download site](https://www.rstudio.com/products/shiny/download-server/) for the latest version of the Shiny server. Because Linux Mint is a derivative of Ubuntu I used that. As of this writing, the most current version is for Ubuntu 14.04 and the Shiny version is 1.5.9.923. From a command line use **wget** to download the installation package.

wget https://download3.rstudio.org/ubuntu-14.04/x86\_64/shiny-server-1.5.9.923-amd64.deb

To install the downloaded package, gdebi is required which is installed by default on my version of Linux Mint. If its not on your system install it from the command line with:

sudo apt-get install gdebi-core

Then as root again, install the Shiny application.

sudo gdebi shiny-server-1.5.9.923-amd64.deb

It will prompt you to accept the install and then take several minutes to complete. After that, if everything goes OK, you'll see a message indicating the server is up and running! Launch a browser and point it to the default URL to see the Welcome page for Shiny Server!

http://localhost:3838

[![](http://edpflager.com/wp-content/uploads/2019/03/WelcomePage-248x300.png)](http://edpflager.com/wp-content/uploads/2019/03/WelcomePage.png)There are two small web apps on the right side of this page, and both should be active when you open the Welcome screen. If not, go back and check to make sure there weren't any problems. A final reminder here: When you install packages in R on Linux Mint by default, it installs those packages under **your** user account's R package library. But they need to be under the Shiny account's package library or preferably under the system-wide R package library so the Shiny account can access them. Be sure to use the syntax I have supplied above to make sure they go into the correct location.