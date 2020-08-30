---
title: R - Quick code to install needed packages
tags:
  - cookbook
  - How-to
  - howto
  - install
  - technical
id: '4111'
categories:
  - - Misc
  - - R
comments: false
date: 2018-10-25 18:37:46
---

[![](http://edpflager.com/wp-content/uploads/2018/09/Rlogo-300x232.png)](http://edpflager.com/wp-content/uploads/2018/09/Rlogo.png)A quick tip this time but a very useful one. If you share your R code with someone, or need to come back to an older piece of code, there are times when a needed package will not be installed on the machine running it. The person will attempt to run the code, and will get an error because of the missing library. To circumvent this, here is a one-line piece of code you can use, that will check for required libraries, and if they are not installed will attempt to install them.  Just substitute the name of the package in the two spots and follow it with a call to load the library:

if( !require(Package\_Name)){ install.packages('Package\_Name')}
library(Package\_Name)

I have tried this with non-CRAN repositories, and it does work. If you do attempt that, be careful when using experimental libraries, because you will need to load any dependencies from the console. It also works with dev\_tools::install\_github inside the braces, to replace the install.packages call, but again, you may need to agree to install dependencies from the console.