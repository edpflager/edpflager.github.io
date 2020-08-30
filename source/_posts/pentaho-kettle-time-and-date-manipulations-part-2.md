---
title: Pentaho Kettle Time and Date Manipulations - Part 2
tags:
  - ETL
  - How-to
  - kettle
  - technical
id: '2178'
categories:
  - - Blog
  - - Business Intelligence
  - - Pentaho
comments: false
date: 2014-07-04 18:50:05
---

[![Transform](http://edpflager.com/wp-content/uploads/2014/07/Transform-300x127.png)](http://edpflager.com/wp-content/uploads/2014/07/Transform.png)When using an ETL tool, one requirement that you will likely run into over and over again will be dealing with dates. In part 1 of this series, I looked at using the GET SYSTEM INFO function in Pentaho which pulled various date and time values based on your current system values. The other step you can add to your Kettle transformations to work with Date and Time values is the Calculator step.

##### Calculator

To add the Calculator function to your ETL process, open the Transform folder in the Design tab, and drag the Calculator function onto the calculator. Double click to open it. This window is a little more complex than the Get System Info step, but not significantly, but that belies the depth of functionality it provides.
<!-- more -->
In the New field box, enter a name for the result you will be creating. Again, you can define multiple items here, so give them meaningful names in the context of your transformation. Skipping the Calculation column momentarily, there are Field A, B and C columns where you specify the parameters that will be used in the calculation. These can be passed from a step in your transformation or you can use the result of a calculation from an earlier line in the window as one of the fields. So for example if you define a New field in step 1  called Test, you can use Test as Field a, B or C in a line 2 or higher. After the Field columns are various options for defining the output such as data types and formatting. There is a column called Remove as well which allows you to keep temporary values from being passed on to the next steps in your transformation. Just set it to Y and those values will be dropped after the Calculator step. The Calculation step is where the bulk of the functionality is in this component. Click on the Calculation field, and you'll be presented with a large number of options as well.  As with the GET SYSTEM INFO function, there are more than just date and time manipulations here. You'll see options for :

*   Basic math functions like square root and rounding, division, multiplication, addition and subtraction.
*   Hash calculations using a variety of checksum algorithms like CRC-32 and MD5
*   XML and HTML manipulation and validation - especially useful is an option to check to see if your XML is well-formed!
*   String and character concatenation, conversion and comparision - remove and add control characters, encoding back and forth with HEX and BYTE, switch case between upper and lower, and number and specific character extractions as well as several steps to compare how similar two strings are.

In a future post, I'll delve into these more in detail. But for more now, lets look at the time and date calculations that are available. They tend to fall into two categories: adding and subtracting to and from a given start date and time, or extracting portions of a given date to get a smaller portion of it such as the day of the week, the hour of the day or even the quarter of the year.

##### DATE/TIME EXTRACTION EXAMPLE

Start a new transformation, and drag the GET SYSTEM INFO step we discussed last time on to the canvas. Open it up, and define a field called GetNow. For the Type, choose system date (variable). Click on the Preview button, and accept the default number of rows and click OK. A new window will open showing the GetNow field has a value set to your current System time.  Click CLOSE and then OK to return to your canvas.[![GetNow](http://edpflager.com/wp-content/uploads/2014/07/GetNow-300x194.png)](http://edpflager.com/wp-content/uploads/2014/07/GetNow.png)

Drag a CALCULATOR step on to your canvas. Connect the two steps by dragging a hop between them. Open the Calculator step and define eight rows, with the fields set to:

*   Year,
*   Hour,
*   Minute,
*   Second,
*   MonthOfYear,
*   DayOfWeek,
*   DayOfMonth, and
*   DayOfYear.

In the field A column for each of your rows, set the value to GetNow (you can click the drop down list and it should be there, or go ahead and type it in). In the Value column, set each row to Integer. The Remove column can be left as blank or set to N. For the calculation column, for the first row, set it to Year of Date A. For the second row, Hour of Day of Date A. For the remaining rows, use:

*   Minute of Hour of Date A,
*   Second of Minute of Date A,
*   Month of Date A,
*   Day of week of date A,
*   Day of Month of date A, and
*   Day of Year of Date A.

When you are done your calculation step should look like the image below. Click OK to return to your canvas.[![Calculator](http://edpflager.com/wp-content/uploads/2014/07/Calculator-300x143.png)](http://edpflager.com/wp-content/uploads/2014/07/Calculator.png) Save your transformation, and go ahead and run it. Switch over to the Preview Data tab in your transformation results panel, and click on the Calculator step. You should see something similar to the image below, with the first column showing the time when the transformation ran and then separate columns for the various calculations.[![Results](http://edpflager.com/wp-content/uploads/2014/07/Results-300x131.png)](http://edpflager.com/wp-content/uploads/2014/07/Results.png) **A couple of things to note here is that the extracted values are integer data types and not time values including the day of the week. For Pentaho, the week starts on Sunday as day 1 and ends on Saturday as Day 7.** PENTAHO  is a registered trademark of Pentaho, Inc.