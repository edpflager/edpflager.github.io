---
title: Pentaho Data Integration's Fuzzy Match
tags:
  - cookbook
  - ETL
  - guides
  - How-to
  - howto
  - kettle
  - PDI
  - SysAdmin
  - technical
id: '2532'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
  - - Pentaho
comments: false
date: 2016-05-26 15:21:54
---

[![fuzzy](http://edpflager.com/wp-content/uploads/2016/05/fuzzy.jpg)](http://edpflager.com/?attachment_id=3311#main)When cleansing data, one of the biggest challenges is determining if one record is the same as another in the absence of a unique identifier. For example, if your database has a record for Terri Lee Duffy, and you get a new record for Terry Lee Duffy, is it the same person? If you have a government ID number then its possible to tell definitively, that its the same person. But what if you don't have that to distinguish the record? You could check other related data if you have it, like street address, but what if one record has 100 South Ave and the other is 100 South Road? A human looking could say yes or no that this is the same person. We don't want to have to check every discrepancy, especially if we are moving millions of rows at a time. In order to automate this process, we can use a component in Pentaho called Fuzzy Match. (For a longer discussion of Fuzzy Matching, [Melissa Data Corporation](https://www.melissadata.com/deduplication/what-is-fuzzy-matching.htm) has a good overview.) While the results of a Fuzzy Match process are not 100% perfect, you can set an allowance threshold so that similarities have to be within a certain range or you can show only the closest match as a result of your Fuzzy Match. Finally, the Fuzzy Match component can use one of several algorithms to determine if one field is a match for another.  The [Pentaho Wiki](http://wiki.pentaho.com/display/EAI/Fuzzy+match) discusses the nuances of these algorithms and has some discussion on the best times to use them.
<!-- more -->
Fuzzy Match Example [![FuzzyTranform](http://edpflager.com/wp-content/uploads/2016/05/FuzzyTranform-300x147.png)](http://edpflager.com/?attachment_id=3298#main) In this example, I am using a Person table with about twenty thousand records, derived from the Microsoft SQL Server AdventureWorks2012 database. (A version for MySQL is available on [SourceForge](https://sourceforge.net/projects/awmysql/)). My source table has a simple query, where the first name, middle name and last name are combined to form a new field, called FullName. This fields will be the one that is actually used to Match on in the component. Here is the query:

SELECT PersonType, FirstName, MiddleName, LastName, 
 FirstName + ' ' + Coalesce(MiddleName,'') + ' ' + LastName as FullName
FROM Person.Person

The NewData component is a DataGrid component. I have defined Meta data of four string fields (FirstName, MiddleName, LastName, and FullName) with three rows of data, pictured below: [![Add constancts](http://edpflager.com/wp-content/uploads/2016/05/Add-constancts-300x192.png)](http://edpflager.com/?attachment_id=3301#main) The first row is an alternative of an existing record with no middle name, the last row is the example I highlighted earlier, changing Terri to Terry in the first name. The middle row is a deliberate misspelling of the last name Smith and there is no existing records with Jane Smith. [![FuzzyStringGeneral](http://edpflager.com/wp-content/uploads/2016/05/FuzzyStringGeneral-300x300.png)](http://edpflager.com/?attachment_id=3305#main)After setting those two inputs up, I join them with the Fuzzy Match component which in PDI is under the Lookup folder in the Design tab. On the General tab of the component, I set my Person source as the Lookup stream. This is the reference side of the function where the data is treated as clean or reliable. FullName is set as my Lookup field. Since I have defined the Person source as my Lookup stream, Pentaho knows that my other input is the Main stream and populates the Main Stream drop down list with the fields available from there. [![FuzzyStringFields](http://edpflager.com/wp-content/uploads/2016/05/FuzzyStringFields-300x298.png)](http://edpflager.com/?attachment_id=3308#main)In the Settings area, I select the algorithm to use for comparing.  In this case I am using the Jano algorithm which is designed to calculate a similarity index between two strings. I skip the case sensitive check box, and check the Get Closer value box. This limits the results for each record checked to the closest match. For minimal value, I enter a .95 because I want matches to be fairly close to my reference table to consider it a match. My maximal value I leave set to 1.0 (an exact match). Switching to the Fields tab, I changed the MatchField name to ReferenceString and the Value field to SimilarityIndex. (Not necessary - but makes it easier to tell what each is). I click the GET FIELDS button to populate the grid with the fields from my Input Stream. Click OK to return to the canvas. Finally I wire up a DUMMY step to stop the flow, and allow me to see the results. Save your transform and Run it accepting the defaults, and after a few moments it will complete. Click on the DUMMY component, and then switch to the PREVIEW DATA tab in the bottom panel will show you what values it has detected as matches for the new data and the similarity between them. [![FuzzyMatchresults](http://edpflager.com/wp-content/uploads/2016/05/FuzzyMatchresults-300x51.png)](http://edpflager.com/?attachment_id=3309#main) Notice that the first row and the last row are shown as a match of 1 (meaning a 100% match). Jane A Smithh shows no results, meaning no matches were found within the 95-100% range that was set. If you reedit the Fuzzy Match component and change the threshold to .90 instead of .95, Jane A Smith is matched to Jacob A Smith at a 90% level. From here you could use the Similarity index as a value to determine if the record is discarded or added as a new row. I'll leave that for you to work with.