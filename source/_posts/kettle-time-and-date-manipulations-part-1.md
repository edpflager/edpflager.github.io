---
title: Pentaho Kettle Time and Date Manipulations - Part 1
tags:
  - ETL
  - How-to
  - kettle
  - technical
id: '2158'
categories:
  - - Blog
  - - Business Intelligence
  - - Pentaho
comments: false
date: 2014-06-30 16:04:12
---

[![Screenshot](http://edpflager.com/wp-content/uploads/2014/06/Screenshot-300x152.png)](http://edpflager.com/wp-content/uploads/2014/06/Screenshot.png)When using an ETL tool, one requirement that you will likely run into over and over again will be dealing with dates. When pulling data from a database, its usually easier and more efficient to use any built-in functions that the database provides. But if you are performing some work in Kettle (AKA Pentaho PDI) jobs that are not part of queries that manipulate dates, you should be able to handle it easily with a couple of Kettle's transform steps. In this part of  this series, we'll look into the GET SYSTEM INFO/DATA function. (The label in the Design | Input tree is GET SYSTEM INFO but when you open the function on the canvas the window is labeled Get System Data.)
<!-- more -->
##### Get System Info

The Get System Info function resides under the INPUT node in the Design tab of the Kettle interface. Drag it onto the canvas and double click it. Its fairly plan, but that hides the abundance of options that are available. Use the name field to provide a name for the item you will be using, generally something meaningful in the context of your transformation. You can add a number of items here, so give them names that can provide some hints to their purpose or what they contain. For my example, I used Yesterday-EndOfDay and Today-StartofDay. Next, click in the Type column and you will be presented with a large list of system information you can use in your transformation. The information falls into several categories: date and time values, machine information, Kettle information, job information and command line arguments. The date and time values are useful when you are creating transformations that may be run regularly because Kettle will substitute the actual value in place of the Information Type you selected. For example:

*   You can define two different values each for Yesterday, Today and Tomorrow - one with a 00:00:00 time component  and the other with a 23:59:59 time component.
*   With the variance in the number of days in a month,  information types for first and last day of last month, this month and next month again with two time components are especially useful.
*   First and last day of last week, this week and next week (with the time components) make manipulating information in week specific chunks easier.
*   Quarter values are also useful, again with first and last day of last quarter, this quarter and next quarter along with the beginning time of 00:00:00 for the quarter and 23:59:59 ending time for the last day of the quarter.
*   First and last day of the previous year, the current year and next year are also handy, allowing you to code transformations that may be used for several years. (And they include the beginning time of 00:00:00 for the year and 23:59:59 for the ending time for the year.)
*   One interesting option is the first day of last week, this week and next week and the last day of the same which has two different versions, one for US and the other blank. In my testing they did not return different results, so I am not sure what the difference is.
*   Finally, there is the Last Working Day of Last week, this Week and Next week. A word of warning - if you use these, be aware that they return Thursday for the last working day of the week, not Friday as it is in the US.

From the example screen shot above, you can see the difference between the start of day and end of day components. The two fields I have created are only a second apart, but the usage of Kettle's defined time variables means I can do some finer grained manipulation without needing to add unnecessary code. Once you have a field defined (or multiple fields), click on the Preview button, and Kettle will show you the results so you can verify you are getting what you expected.

##### COMING UP...

As I stated above, the Get System Info component also provides a large number of other predefined values that may be useful in your ETL workflows. We'll delve in to those at a future date. Next time we'll look at another component that Pentaho provides to allow you to define and manipulate date and time functions, and how to perform calculations using the fields we've defined here, and more.