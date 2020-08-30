---
title: Formulas in R Markdown
tags:
  - How-to
  - R Markdown
  - technical
id: '4131'
categories:
  - - Business Intelligence
  - - Misc
  - - R
  - - R Markdown Cookbook
comments: false
date: 2018-11-06 18:02:07
---

[![](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)](http://edpflager.com/wp-content/uploads/2018/09/rmarkdown-e1538176415459.png)R Markdown is used to create documents to show the analysis that data was put through and the results of that analysis in a one document. But what if you need to include the formula notation for a calculation in your document? While the common format for a calculation may be well known and understood, reproducing it may be difficult because of mathematical symbols that aren't generally included in a computer font set. In the interest of thoroughness though, the notation should be included. I will preface the rest of this article with this note: some of this works with HTML, Word and PDF. However if you have nested formula notations like I do on the Variance section, some of it may not work properly when using Word output.

### Pythagorean theorem

[![](http://edpflager.com/wp-content/uploads/2018/11/theorem-266x300.png)](http://edpflager.com/wp-content/uploads/2018/11/theorem.png)Let me illustrate first with a common formula used in geometry: the Pythagorean theorem. In its simplest form, it states that for a right triangle, summing the squares of the sides of the triangle adjacent to the right angle equals the square of the side opposite the right angle (hypotenuse). As a refresher, this is usually notated as I have shown here. For the most part this is a fairly easy formula to reproduce on a computer keyboard. The letters are no problem, the + and = symbols are readily accessible. However including the superscripts that represent the square of the values a, b and c may require a little digging. For Windows: Ctrl and Shift, then press + may work depending on the application your are using. Mac OS X it may work by pressing the Command, Option, Shift  and + keys. To recreate it with R Markdown, you type:

$a^{2} + b^{2} = c^{2}$

and when you knit your document, it produces the familiar notation. But additionally with R you can have R do the calculation for you, by setting the variables a and b and defining the calculation. Then if the variables change, the calculation will also change when the document is passed to KNIT again. The code can be entered like this in an R Markdown document:

\`\`\`{r echo=FALSE}
a<-2
b<-4
\`\`\`

$a =$ \`r a\`, $b =$ \`r b\`, $c =$ ? ## Display values

$a^{2} + b^{2} = c^{2}$ ## Display formula to plug values into

$a^{2} + b^{2} =$ \`r a^2 + b^2\` ## Show the result of first calculation

\`r a^2 + b^2\` = $c^{2}$ ## Result is equal to $c^{2}$

$sqrt(\`r a^2 + b^2\`)$ = \`r sqrt(a^2 + b^2)\` ## Get the square root

$c =$ \`r sqrt(a^2 + b^2)\` ## Value of the hypothenuse

The first section defines your a and b values but does not echo that to the document.  The resulting Markdown document looks like this (without the comments): [![](http://edpflager.com/wp-content/uploads/2018/11/theorumResults-300x274.png)](http://edpflager.com/wp-content/uploads/2018/11/theorumResults.png)

### Variance and Standard Deviation of a Sample

Let's do one more example, calculating the variance and standard deviation of a sample data set. As a refresher, both measures tell us how spread out our data set is, but are expressed in different ways. Variance measures the average degree each data point differs from the mean of the elements in the  data set. Standard deviation is the square root of the variance and restores it to the same unit of measure as the original data set. There as slight differences in calculating variance and standard deviation depending on whether you are working with the entire population or just a sample of data. The mathematical notation for a variance can be shown in an R Markdown document using the formula below and is displayed in the second figure:

$sigma^{2}$ = $frac{sum((x - bar{x})^{2})}{n-1}$

[![](http://edpflager.com/wp-content/uploads/2018/11/variance.png)](http://edpflager.com/wp-content/uploads/2018/11/variance.png) Showing the steps in our work, we need to define our sample data set and calculate the variance and standard deviation with built-in R functions to use as a check later in our document.

sm <- c(4,5,6,7,8,9)
variance(sm) = 3.5 
standard deviation(sm) = 1.8708287

We go through several other steps in the R Markdown document to define the components that lead to the variance and standard deviation outcome. The full code for this section is at the end of this post, but here is a sample of the PDF output that incorporates text, inline R code, R code chunks and formulas: [![](http://edpflager.com/wp-content/uploads/2018/11/VarianceMixed-1024x296.png)](http://edpflager.com/wp-content/uploads/2018/11/VarianceMixed.png)

* * *

\---
title: "MathFormulas"
output:
   pdf\_document
---

\`\`\`{r setup, include=FALSE}
knitr::opts\_chunk$set(echo = TRUE)
\`\`\`
Works in PDF, Word or HTML (generally)

All equations have to be enclosed between dollar ($) signs. If you have matching ones, in R Studio, by default they will turn red unless you use them inline. You can also have formulas in line, like this $sqrt(2)$ , or on a separate line like this:

$sqrt(2)$

Some examples:
$2^{2}$

$sum\_(z=c)^{a}$

$a^{2} + b^{2} = c^{2}$

$frac{5}{3}$

If you want the formula to be larger and stand out from the text, you can use presentation mode, by enclosing your formula with two dollar signs on either side. It will also center the text in the output:

$$sum$$


\`\`\`{r echo=FALSE}
a<-2 ## define side a
b<-4 ## define side b
\`\`\`

Pythagorean theorem example using R formula nominclature

$a =$ \`r a\`, $b =$ \`r b\`, $c =$ ? 

$a^{2} + b^{2} = c^{2}$

$a^{2} + b^{2} =$ \`r a^2 + b^2\`

\`r a^2 + b^2\` = $c^{2}$

$sqrt(\`r a^2 + b^2\`)$ = \`r sqrt(a^2 + b^2)\`

$c =$ \`r sqrt(a^2 + b^2)\`

* * *

\---
title: "MathFormulas2"
output:
   pdf\_document
---

##Calculate variance for a sample, in mathematical notation or using R code:

$sigma^{2}$ = $frac{sum((x - bar{x})^{2})}{n-1}$ OR $sigma^{2}$ = (sum((sm-mean(sm))^2))/(length(sm)-1)

## Taking the square root of the variance is the standard deviation for the sample

$sigma$ = $sqrtfrac{sum((x - bar{x})^{2})}{n-1}$

##Define a sample data set and calculate the variance and standard deviation with built-in R functions as a check on our calculations.
\`\`\`{r echo=FALSE} 
sm <- c(4,5,6,7,8,9)

sm\_mean <- mean(sm)

sm\_size <- length(sm) - 1

sm\_sum = sum((sm-mean(sm))^2)

\`\`\`

sm <- c(4,5,6,7,8,9)

variance(sm) = \`r var(sm)\` and standard deviation(sm) = \`r sd(sm)\`

## Now calculate the various parts, starting with the sample mean: $bar{x}$ 
(mean(sm)) = \`r mean(sm)\`

## Get the total number of dataset elements minus one: n-1 
sm\_size = length(sm)- 1 = \`r length(sm)-1\`

## Find the difference between the data set mean and each element, square it, and sum them: $sum((x - bar{x})^{2})$ 
sm\_sum = sum((sm-mean(sm))^2) = \`r sum((sm-mean(sm))^2)\`

## Calculate the variance using the values: $frac{sum((x - bar{x})^{2})}{n-1}$ 
(sm\_sum / sm\_size) = \`r sm\_sum / sm\_size\`

## Standard Deviation equals the square root of the variance: $sqrtfrac{sum((x - bar{x})^{2})}{n-1}$
sqrt((sm\_sum / sm\_size)) = \`r sqrt((sm\_sum / sm\_size))\`

## Putting it all together into one line of R code, we see that the variance is the same using the built in function or by calculating the various components.
(sum((sm-mean(sm))^2))/(length(sm)-1) = \`r (sum((sm-mean(sm))^2))/(length(sm)-1)\`