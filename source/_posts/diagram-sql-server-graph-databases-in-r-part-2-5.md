---
title: Diagram SQL Server Graph Databases in R - Part 2.5
tags:
  - graph database
  - howto
  - metadata
  - SQL Server
  - SysAdmin
  - technical
id: '4403'
categories:
  - - Blog
  - - Business Intelligence
  - - R
comments: false
date: 2019-02-06 18:40:26
---

![](http://edpflager.com/wp-content/uploads/2019/02/cursorwithschema-300x55.png)Quick post tonight. While working on some examples to test out the cursor I posted in Part 2, I realized that I had only coded the cursor to retrieve information for the dbo schema. So here I present the revised cursor which pulls from any schema. If a table is part of the dbo schema its not used in the results, since that is the normal default:
<!-- more -->
 

SET nocount on;

IF OBJECT\_ID('tempdb.dbo.#NodeEdgeXRef', 'U') IS NOT NULL

DROP TABLE ##NodeEdgeXRef;

CREATE TABLE ##NodeEdgeXRef (FromNode INT, ToNode INT, EdgeID Int);

DECLARE @NodeID as INT;
DECLARE @NodeName as NVARCHAR(128);
DECLARE @EdgeID as INT;
DECLARE @SchemaName as NVARCHAR(128);
DECLARE @sSQL as nvarchar(MAX);
DECLARE @NodeCursor as CURSOR;

/\* Revised to include schema here \*/

SET @NodeCursor = CURSOR FOR
SELECT  tb.name, tb.object\_id, tb.object\_id, sc.name
FROM    sys.tables tb
JOIN      sys.schemas sc ON tb.schema\_id = sc.schema\_id
WHERE  is\_edge = 1;

OPEN @NodeCursor;

FETCH NEXT FROM @NodeCursor INTO @NodeName, @NodeID, @EdgeID, @SchemaName;

/\* Revised to include SCHEMA here \*/

WHILE @@FETCH\_STATUS = 0
BEGIN

set @sSQL=N'select distinct object\_id\_from\_node\_id($from\_id) as FromNode,
object\_id\_from\_node\_id($to\_id) as ToNode,'
+ CAST(@EdgeID as NVARCHAR(MAX))+ ' as EdgeID from ' + @SchemaName + '.' +@NodeName;

INSERT INTO ##NodeEdgeXRef

EXEC sp\_executesql @sSQL;

FETCH NEXT FROM @NodeCursor INTO @NodeName, @NodeID, @EdgeID, @SchemaName;

END

CLOSE @NodeCursor;

DEALLOCATE @NodeCursor;


/\* Revised query to add schema \*/

SELECT distinct 
        CASE WHEN frs.name = 'dbo' THEN fr.name
            ELSE frs.name +'.'+ fr.name 
        END as FromName, 
        FromNode, 
        CASE WHEN es.name = 'dbo' THEN en.name
            ELSE es.name +'.'+ en.name 
        END as EdgeName, 
        EdgeID,
        CASE WHEN ts.name = 'dbo' THEN tn.name
             ELSE ts.name + '.' + tn.name 
        END as ToName, 
        ToNode
FROM ##NodeEdgeXRef xr
JOIN sys.tables fr ON xr.FromNode = fr.object\_id 
JOIN sys.tables tn ON xr.ToNode = tn.object\_id
JOIN sys.tables en ON xr.EdgeID = en.object\_id
JOIN sys.schemas frs ON fr.schema\_id = frs.schema\_id
JOIN sys.schemas ts ON tn.schema\_id = ts.schema\_id
JOIN sys.schemas es ON en.schema\_id = es.schema\_id;

DROP TABLE ##NodeEdgeXRef;