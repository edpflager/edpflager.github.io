---
title: RStudio on Linux Mint 19
tags:
  - guides
  - How-to
  - howto
  - install
  - Mint
  - RStudio
id: '4269'
categories:
  - - Blog
  - - Business Intelligence
  - - Misc
  - - R
comments: false
date: 2018-12-17 19:34:02
---

[![](http://edpflager.com/wp-content/uploads/2018/12/RStudio-e1545081980313.png)](http://edpflager.com/wp-content/uploads/2018/12/RStudio-e1545081980313.png)R Studio is a popular IDE to use with R and runs on Windows (7/8/10), Mac OS X (10.6+) and the most widely used Linux distributions: Debian/Ubuntu and Fedora/Red Hat/OpenSuSE. For Linux, that generally means installation on any other distributions that are derivatives of these should also work with with minimal effort. For my personal Linux efforts, I have bounced between CentOS (a derivative of Red Hat), and Linux Mint (version 19 built from Ubuntu 18.04). Recently I picked up a refurbished Acer Swift 1 (cheap!) so I would have a Linux laptop to work with. The Comprehensive R Archive Network (CRAN) has Ubuntu installation packages of R-base up through Ubuntu Cosmic 18.10. Before installing RStudio, you need to install base-R the minimum installation of R. Here is instructions for that based on the [CRAN website](https://cran.r-project.org/bin/linux/ubuntu/).
<!-- more -->
1.  Add the following line to your /etc/apt/sources.list file by editing it as sudo.
    
    deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran35/
    
2.  Import the ubuntu apt secure key for the CRAN repository by executing this statement:
    
    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
    
3.  Update your repository list:
    
    sudo apt-get update
    
4.  Install the base version of R
    
    sudo apt-get install r-base
    
5.  Start R by typing a capital letter R and pressing Enter, and you should be greeted with the startup message for R telling you the version number.
6.  Follow the onscreen instructions to exit R and answer n to the save workspace command to exit back to the command line.
7.  Congratulations you have a working installation of R!

Now we want to install RStudio so we have a graphical IDE for working with R. Ubuntu packages for RStudio are only available through 16.04 Xenial but you can install them in Linux Mint 19 if you install a dependency first.

1.  Back at the command line, make sure to update your repository list again:
    
    sudo apt-get update
    
2.  Install any new versions:
    
    sudo apt-get upgrade
    
3.  Now install the GStreamer library:
    
    sudo apt-get install libgstreamer1.0
    
4.  Open a web browser, and download the latest Debian/Ubuntu version of RStudio from [rstudio.com](http://rstudio.com/).
5.  You can install the package by opening the file manager in Mint, and double clicking the package. This will open the GDebi Package Installer application, which will inform you that one dependency is needed.[![](http://edpflager.com/wp-content/uploads/2018/12/GDebi-300x290.png)](http://edpflager.com/wp-content/uploads/2018/12/GDebi.png)
6.  Click the DETAILS button and then click on the Details window to proceed, and when you return to the main GDebi window, click Install Package.![](http://edpflager.com/wp-content/uploads/2018/12/RStudioDependency-300x198.png)
7.  When the installation completes, click on the Mint Menu icon in the lower left corner of the screen. Navigate to the Programming folder, and you should now see RStudio listed. Click on it, and the application will open.

Ta-da!