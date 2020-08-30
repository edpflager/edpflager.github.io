---
title: Diagram SQL Server Graph Databases in R - Part 1
tags:
  - Graph Databases
  - howto
  - metadata
  - SQL Server
  - SysAdmin
  - technical
id: '4338'
categories:
  - - Business Intelligence
  - - Linux
  - - Misc
  - - R
comments: false
date: 2019-01-29 15:46:52
---

[![](http://edpflager.com/wp-content/uploads/2019/01/student-class.png)](http://edpflager.com/wp-content/uploads/2019/01/student-class.png)

Welcome to the first of several articles covering generating diagrams in R of Microsoft SQL Server graph databases. A little theoretical background is in order before diving into the mechanics. In relational databases, tables typically connect or join based on a unique value in one table to another table where the same value may appear one or more times. Called a one-to-one or one-to-many relationship, it functions very well for most purposes. Dealing with real world situations with many-to-many relationship representation has been more awkward. As an example, the image above shows a representation of a classic many-to-many situation: class enrollment. A single student may be enrolled in many classes, and a single class may contain many students. In order to model this in a relational database, you would need to use an intermediary table, with one row for each student and each class they are enrolled in. By doing it this way, you can query the tables in either direction to get a list of all the students with the classes they are enrolled in, or all of the classes and which students are enrolled in those classes.
<!-- more -->
While graph databases have existed almost as long as relational databases, its only since the advent of NoSQL databases that they have become more widespread. An introduction to Graph databases with all of its nuances is beyond the scope of this series, but for those who are interested, Neo4J, Inc (manufacturer of the most popular graph databases systems) provides a [free ebook](https://neo4j.com/graph-databases-book/?ref=home) covering many Graph database concepts. [![](http://edpflager.com/wp-content/uploads/2019/01/class_graph-300x132.png)](http://edpflager.com/wp-content/uploads/2019/01/class_graph.png)For purposes of these articles, its enough to say that Graph databases seek to model the information by expressing the relationship as part of the model. So using the student class example, a graph database design could be visualized like the figure here: Note that the enrollment relationship shows arrows on both ends, meaning that its bidirectional. Students can be enrolled in a class, and a classes can have many students enrolled. Some situations don't make sense to have a bidirectional relationship, say for example ownership of a book. A person may own many books in their lifetime, but an individual books doesn't belong to many people. For bidirectional relationships, use arrows on both ends of the relationship, otherwise just arrows in one direction.

##### USING MICROSOFT SQL SERVER

In [SQL Server 2017](https://docs.microsoft.com/en-us/sql/relational-databases/graphs/sql-graph-overview?view=sql-server-2017), Microsoft started incorporating graph processing technology. While still new and not without some warts, it does hold some promise and the basic functionality is sufficient for what I wanted to accomplish here. Often times when working on a project, I don't have access to the underlying OLTP database model and have to do a lot of discovery to understand how data is processed through the system.  A visual representation of the graph tables is a quick way to understand the relationships and the entities in a graph database. The initial genesis of this series was to show how to generate a visualization of a SQL Server graph database to provide that understanding. It grew into producing a reusable process to generate visualizations from any Microsoft graph database just from knowing the name of the database. For initial testing, here is the code to create a simple graph database, based on the Student <- enrolled -> Class model.

CREATE DATABASE StudentClass;
GO

USE StudentClass;
GO

--Create Node tables and populate them
CREATE TABLE Student(
StudentID int PRIMARY KEY,
StudentName varchar (100) 
) as NODE;

INSERT INTO Student Values (1, 'Will Shakespeare');
INSERT INTO Student Values (2, 'Percy Shelley');
INSERT INTO Student Values (3, 'Chuck Dickens');
INSERT INTO Student Values (4, 'Art Doyle');
INSERT INTO Student Values (5, 'Gina Wolfe');

CREATE TABLE Class(
ClassID int PRIMARY KEY,
ClassName varchar(100)
) as NODE;

INSERT INTO Class Values (1, 'History of Surfing');
INSERT INTO Class Values (2, 'Joy of Garbage');
INSERT INTO Class Values (3, 'Art of Walking');
INSERT INTO Class Values (4, 'Street Fighting Mathematics');
INSERT INTO Class Values (5, 'Fermentation Studies');

--Create EDGE table and populate it.
CREATE TABLE enrolledIn as Edge;

INSERT INTO enrolledIn VALUES ((SELECT $node\_id FROM Student WHERE StudentID = 1),
(Select $node\_id from Class WHERE ClassID = 1));
INSERT INTO enrolledIn VALUES ((SELECT $node\_id FROM Student WHERE StudentID = 1),
(Select $node\_id from Class WHERE ClassID = 2));
INSERT INTO enrolledIn VALUES ((SELECT $node\_id FROM Student WHERE StudentID = 1),
(Select $node\_id from Class WHERE ClassID = 3));
INSERT INTO enrolledIn VALUES ((SELECT $node\_id FROM Student WHERE StudentID = 2),
(Select $node\_id from Class WHERE ClassID = 3));
INSERT INTO enrolledIn VALUES ((SELECT $node\_id FROM Student WHERE StudentID = 2),
(Select $node\_id from Class WHERE ClassID = 4));
INSERT INTO enrolledIn VALUES ((SELECT $node\_id FROM Student WHERE StudentID = 3),
(Select $node\_id from Class WHERE ClassID = 1));
INSERT INTO enrolledIn VALUES ((SELECT $node\_id FROM Student WHERE StudentID = 3),
(Select $node\_id from Class WHERE ClassID = 4));
INSERT INTO enrolledIn VALUES ((SELECT $node\_id FROM Student WHERE StudentID = 3),
(Select $node\_id from Class WHERE ClassID = 5));
INSERT INTO enrolledIn VALUES ((SELECT $node\_id FROM Student WHERE StudentID = 4),
(Select $node\_id from Class WHERE ClassID = 2));
INSERT INTO enrolledIn VALUES ((SELECT $node\_id FROM Student WHERE StudentID = 4),
(Select $node\_id from Class WHERE ClassID = 5));
INSERT INTO enrolledIn VALUES ((SELECT $node\_id FROM Student WHERE StudentID = 5),
(Select $node\_id from Class WHERE ClassID = 3));

This code creates two Node tables (Student and Class), and the Edge table that bridges them (enrolledIn). Sample data for five students, five classes, and who is enrolled in each class is added. **UPDATE: I had forgotten to include the data to make  the edge going in the opposite direction from the above. Here is the additional code to set that up:**

CREATE TABLE enrolledStudent as Edge;
INSERT INTO enrolledStudent VALUES ((Select $node\_id from Class WHERE ClassID = 1),
 (SELECT $node\_id FROM Student WHERE StudentID = 1));
INSERT INTO enrolledStudent VALUES ((Select $node\_id from Class WHERE ClassID = 1),
 (SELECT $node\_id FROM Student WHERE StudentID = 3));
INSERT INTO enrolledStudent VALUES ((Select $node\_id from Class WHERE ClassID = 2),
 (SELECT $node\_id FROM Student WHERE StudentID = 1));
INSERT INTO enrolledStudent VALUES ((Select $node\_id from Class WHERE ClassID = 2),
 (SELECT $node\_id FROM Student WHERE StudentID = 4));
INSERT INTO enrolledStudent VALUES ((Select $node\_id from Class WHERE ClassID = 2),
 (SELECT $node\_id FROM Student WHERE StudentID = 3));
INSERT INTO enrolledStudent VALUES ((Select $node\_id from Class WHERE ClassID = 3),
 (SELECT $node\_id FROM Student WHERE StudentID = 1));
INSERT INTO enrolledStudent VALUES ((Select $node\_id from Class WHERE ClassID = 3),
 (SELECT $node\_id FROM Student WHERE StudentID = 2));
INSERT INTO enrolledStudent VALUES ((Select $node\_id from Class WHERE ClassID = 4),
 (SELECT $node\_id FROM Student WHERE StudentID = 2));
INSERT INTO enrolledStudent VALUES ((Select $node\_id from Class WHERE ClassID = 4),
 (SELECT $node\_id FROM Student WHERE StudentID = 3));
INSERT INTO enrolledStudent VALUES ((Select $node\_id from Class WHERE ClassID = 5),
 (SELECT $node\_id FROM Student WHERE StudentID = 3));
INSERT INTO enrolledStudent VALUES ((Select $node\_id from Class WHERE ClassID = 5),
 (SELECT $node\_id FROM Student WHERE StudentID = 4));

We now have a functional, but simple graph database. Next time, I'll cover briefly some of the new system functions that were added to SQL Server to allow you to query for the structure of graph databases, and present code I created that will show that information in a handy format for review.