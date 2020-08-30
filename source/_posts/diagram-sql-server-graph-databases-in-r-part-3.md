---
title: Diagram SQL Server Graph Databases in R - Part 3
tags:
  - graph database
  - howto
  - metadata
  - SQL Server
  - technical
id: '4415'
categories:
  - - Blog
  - - Business Intelligence
  - - R
comments: false
date: 2019-02-10 12:17:40
---

[![](http://edpflager.com/wp-content/uploads/2019/02/odbc2R-300x108.png)](http://edpflager.com/wp-content/uploads/2019/02/odbc2R.png)The first 2.5 parts of this series ([part 1](http://edpflager.com/2019/01/29/diagram-sql-server-graph-databases-in-r-part-1/), [part 2](http://edpflager.com/2019/02/02/diagram-sql-server-graph-databases-in-r-part-2/), [part 2.5](http://edpflager.com/2019/02/07/diagram-sql-server-graph-databases-in-r-part-2-5/))  focused on setting up a graph database in SQL Server, and querying the metadata to discover the nodes and edges that define the structure of the graph. In this part, we shift over to R and imbed the SQL Server cursor script to retrieve data for manipulation. As a prerequisite, on whatever operating system you are using, you need to install a Microsoft SQL Server ODBC driver. The Microsoft website has easy to follow tutorials for Mac OS X and Linux [here](https://docs.microsoft.com/en-us/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server?view=sql-server-2017) and [here](https://docs.microsoft.com/en-us/sql/connect/odbc/windows/microsoft-odbc-driver-for-sql-server-on-windows?view=sql-server-2017) for Windows. You will also need to install the [ODBC](https://cran.r-project.org/web/packages/odbc/index.html) package in R for this code to work as well, so follow normal R package instructions for that.
<!-- more -->
Once you have your R environment setup, start a new R script/project for the Graph Database diagram. Copy the code below into your script. Its basically the same script that was used in the cursor with a few changes to allow it to work with R. The biggest change involves how R parses strings. Do include a quote character in your string, you must proceed it with the   (slash) character. I have added comments in the script to document what each section is doing:

\## Load ODBC library and create connection to SQL Server. Replace the Uid and PWD fields with your UserID and Password info. 
library(odbc)

con <- dbConnect(odbc::odbc(), 
       .connection\_string = "Driver={ODBC Driver 17 for SQL Server};Server=localhost;Database=StudentClass;Uid=sa;Pwd=\*\*\*\*\*\*\*\*;")

## Define a data frame to hold the query and pass it the query.
## Set Nocount On is necessary for R ODBC to return results from a temporary table.

nodes\_edges <- dbGetQuery(con, 'SET nocount on;

        ## Check to see if the temporary table is present. If it is, drop it before starting
        IF OBJECT\_ID('tempdb.dbo.##NodeEdgeXRef','U') IS NOT NULL
        DROP TABLE ##NodeEdgeXRef;

        ## Create a temporary table to hold the various table ID values.
        CREATE TABLE ##NodeEdgeXRef (FromNode INT, ToNode INT, EdgeID Int);

        ## Declare the Cursor variables.
        DECLARE @NodeID as INT;
        DECLARE @NodeName as NVARCHAR(128); 
        DECLARE @EdgeID as INT;
        DECLARE @SchemaName as NVARCHAR(128);
        DECLARE @sSQL as nvarchar(MAX);
        DECLARE @NodeCursor as CURSOR;

        ## Define the query used in the Cursor.
        SET @NodeCursor = CURSOR FOR
           select tb.name, tb.object\_id, tb.object\_id, sc.name
           from sys.tables tb
           JOIN sys.schemas sc ON tb.schema\_id = sc.schema\_id 
           where is\_edge = 1; 

        ## Open the Cursor and fetch the first resultset.
        OPEN @NodeCursor;
        FETCH NEXT FROM @NodeCursor INTO @NodeName, @NodeID, @EdgeID, @SchemaName;

        ## While the fetch of the last resultset was successful, here is the code to execute.
        WHILE @@FETCH\_STATUS = 0
        BEGIN
           set @sSQL=N'select distinct object\_id\_from\_node\_id($from\_id) as FromNode,
                        object\_id\_from\_node\_id($to\_id) as ToNode,'
                        +CAST(@EdgeID as NVARCHAR(MAX))+' as EdgeID from ' +@SchemaName + '.' + @NodeName;

        ## Store the results in the temporary table
        INSERT INTO ##NodeEdgeXRef 

        ## Execute the code
        EXEC sp\_executesql @sSQL;

        ## Repeat
        FETCH NEXT FROM @NodeCursor INTO @NodeName, @NodeID, @EdgeID, @SchemaName;

        END

        ## Close the cursor and deallocate it.
        CLOSE @NodeCursor;
        DEALLOCATE @NodeCursor; 

        ## Get the data from the temporary table, dropping the dbo schema if present, and returning it to R.
        SELECT distinct 
               CASE WHEN frs.name = 'dbo' THEN fr.name
                    ELSE frs.name + '.' + fr.name 
                 END as FromName, 
               FromNode,
               CASE WHEN es.name = 'dbo' THEN en.name
                    ELSE es.name + '.' + en.name 
                 END as EdgeName, 
               EdgeID, 
               CASE WHEN ts.name = 'dbo' THEN tn.name
                    ELSE ts.name +'.' + tn.name 
                 END as ToName, 
               ToNode
         FROM ##NodeEdgeXRef xr
         JOIN sys.tables fr ON xr.FromNode = fr.object\_id 
         JOIN sys.tables tn ON xr.ToNode = tn.object\_id
         JOIN sys.tables en ON xr.EdgeID = en.object\_id
         JOIN sys.schemas frs ON fr.schema\_id = frs.schema\_id
         JOIN sys.schemas ts ON tn.schema\_id = ts.schema\_id
         JOIN sys.schemas es ON en.schema\_id = es.schema\_id; 

         ## Drop the temporary table
         DROP TABLE ##NodeEdgeXRef;
        ')

        View(nodes\_edges);

Executing this code, should return a data.frame holding the names and tables IDs of the FROM and TO nodes, and the EDGE table name and ID. The View statement at the end will produce something similar to this, if you are using R Studio. (Your table IDs will vary). [![](http://edpflager.com/wp-content/uploads/2019/02/NodesEdges.png)](http://edpflager.com/wp-content/uploads/2019/02/NodesEdges.png) Almost complete now, next time I will share the R code that generates a Graph database diagram like this to illustrate the interrelatedness of our sample graph database: [![](http://edpflager.com/wp-content/uploads/2019/02/StudentClass-300x172.png)](http://edpflager.com/wp-content/uploads/2019/02/StudentClass.png)