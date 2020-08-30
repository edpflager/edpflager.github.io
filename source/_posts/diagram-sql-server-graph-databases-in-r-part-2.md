---
title: Diagram SQL Server Graph Databases in R - Part 2
tags:
  - graph database
  - guides
  - howto
  - metadata
  - SQL Server
  - SysAdmin
id: '4379'
categories:
  - - Business Intelligence
  - - Linux
  - - R
comments: false
date: 2019-02-02 13:44:57
---

This is part two of a series about diagramming SQL Server graph databases in R. [In the first part](http://edpflager.com/2019/01/29/diagram-sql-server-graph-databases-in-r-part-1/), I covered setting up a simple graph database in SQL Server, with just a handful of tables to use as a test bed for this series. The database had one edge (enrolledIn). I have revised the code to include a second edge that goes in the opposite direction, called enrolledStudent, and  that code has been appended to the end of the first part and is reproduced here as well:

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
<!-- more -->
##### GRAPH DATABASE ADDITIONS TO SQL SERVER

When creating graph database tables, SQL Server generates a unique node\_id and edge\_id for the relevant tables. To make querying easier, generic functions **$node\_id** and **$edge\_id** have been added to T-SQL allowing for reusable code without having to determine that unique ID. You can see the **$node\_id** being used in the code for the DML for the sample database. Here is a screenshot of the **enrolledIn** edge table on my system. Note that the edge\_ID field is really just a substitute for a PK and the data for the from\_ID and to\_ID fields are essentially FKs to help in locating the relevant records in the node tables. The $node\_id and $edge\_id functions will be used later to identify the relationships between the graph tables. [![](http://edpflager.com/wp-content/uploads/2019/02/enrolledIn.png)](http://edpflager.com/wp-content/uploads/2019/02/enrolledIn.png) In SQL Server 2017, Microsoft modified some of the system views and functions that provide information on the various components in a database to support the new graph functionality. This [Microsoft webpage](https://docs.microsoft.com/en-us/sql/relational-databases/graphs/sql-graph-architecture?view=sql-server-2017#metadata) provides a considerable amount of information for those interested in learning more. For the purpose of this article, I am working only with a subset of the new information provided. Specifically additional information added to **sys.tables** where tables now have two new bit datatype columns: **is\_node** and **is\_edge,** and the **node\_id$** and **edge\_id$** information added to the edge tables. When querying the sys.tables view for a table, if **is\_node** = 1, the table is a node. If **is\_edge** = 1, the table is an edge. If both values are zero, then the table is neither an edge or a node. The values cannot be 1 for both fields. After creating the database StudentClass and the tables from the provided code, I ran the query below to see the **sys.tables** information, which identifies which tables are nodes and edges. For databases with non graph tables, a WHERE clause excluding tables where is\_edge and is\_node are both zero would replicate the same functionality:

Use StudentClass;
go
select name, object\_id, is\_edge, is\_node from sys.tables
where type = 'U';
![](http://edpflager.com/wp-content/uploads/2019/02/systable.png)

Six new system functions were also added to SQL Server to provide information on graph database components.  Of these, I used two  to determine the information needed to map out the structure of the graph database. **OBJECT\_ID\_FROM\_NODE\_ID** will take a $node\_id as input and will return the object\_id of the table that the node belongs to. **OBJECT\_ID\_FROM\_EDGE\_ID** is similar, returning the object\_id of a table that and edge belongs to.

##### CURSOR TO ACQUIRE THE GRAPH DATA

[![](http://edpflager.com/wp-content/uploads/2019/02/NodeEdgeStudent.png)](http://edpflager.com/wp-content/uploads/2019/02/NodeEdgeStudent.png)

Using the information from the **sys.table** view and the two system functions discussed above, I was able to create a cursor in TSQL to generate the above output. As you can see, it shows that there are two paths in this graph database:

**Student -> (enrolledIn) -> Class and Class -> (enrolledStudent) -> Student.** 

Because the edge table acts like a cross reference between the node tables, the cursor only needs to get information from the edge table for the initial compilation of information. All of the data is stored in a temporary table so I can return one data set rather than a separate data set for each node table once everything has been pulled. The completed code for the cursor is shown below:

SET nocount on;

IF OBJECT\_ID('tempdb.dbo.#NodeEdgeXRef', 'U') IS NOT NULL
DROP TABLE ##NodeEdgeXRef;

CREATE TABLE ##NodeEdgeXRef (FromNode INT, ToNode INT, EdgeID Int);

DECLARE @NodeID asINT;
DECLARE @NodeName asNVARCHAR(128);
DECLARE @EdgeID asINT;
DECLARE @sSQL asnvarchar(MAX);
DECLARE @NodeCursor as CURSOR;

SET @NodeCursor = CURSOR FOR
   select name, object\_id, object\_id
   from sys.tables
   where is\_edge =1;

OPEN @NodeCursor;

FETCH NEXT FROM @NodeCursor INTO @NodeName, @NodeID, @EdgeID;

WHILE@@FETCH\_STATUS=0
  BEGIN
     set @sSQL=N'select distinct object\_id\_from\_node\_id($from\_id) as FromNode,
                 object\_id\_from\_node\_id($to\_id) as ToNode,'
                 +CAST(@EdgeID as NVARCHAR(MAX))+' as EdgeID from '+@NodeName;

      INSERT INTO ##NodeEdgeXRef

      EXEC sp\_executesql @sSQL;

      FETCH NEXT FROM @NodeCursor INTO @NodeName, @NodeID, @EdgeID;

END

CLOSE @NodeCursor;

DEALLOCATE @NodeCursor;

SELECT fr.name as FromName, FromNode,en.name as EdgeName, EdgeID, tn.name as ToName, ToNode
FROM ##NodeEdgeXRef xr
JOIN sys.tables fr ON xr.FromNode = fr.object\_id
JOIN sys.tables tn ON xr.ToNode = tn.object\_id
JOIN sys.tables en ON xr.EdgeID = en.object\_id;

DROP TABLE ##NodeEdgeXRef;

Next time, we'll look at dropping this cursor into an R script and then using the R package DiagrammeR to generate a visualization of the database structure!