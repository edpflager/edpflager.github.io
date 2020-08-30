---
title: Diagram SQL Server Graph Databases in R - Part 4
tags:
  - graph database
  - howto
  - SQL Server
  - SysAdmin
  - technical
id: '4432'
categories:
  - - Big Data
  - - Blog
  - - Business Intelligence
  - - R
comments: false
date: 2019-02-13 14:44:27
---

[![](http://edpflager.com/wp-content/uploads/2019/02/graphdemo.png)](http://edpflager.com/wp-content/uploads/2019/02/graphdemo.png)Here we are at the last part for this series. To recap, in the first 2.5 parts of this series ([part 1](http://edpflager.com/2019/01/29/diagram-sql-server-graph-databases-in-r-part-1/), [part 2](http://edpflager.com/2019/02/02/diagram-sql-server-graph-databases-in-r-part-2/), [part 2.5](http://edpflager.com/2019/02/07/diagram-sql-server-graph-databases-in-r-part-2-5/)) I focused on setting up a graph database in SQL Server, and querying the metadata to discover the nodes and edges that define the structure of the graph. In the [third part](http://edpflager.com/2019/02/10/diagram-sql-server-graph-databases-in-r-part-3/), I shifted over to R and embedded the SQL Server cursor script in an R file to retrieve data for manipulation. Now with this final section, I'll show you how to use Rich Iannone's [DiagrammeR](http://rich-iannone.github.io/DiagrammeR/) package to generate a graph visualization. At the left is a visualization I created of a Microsoft sample graph database that I used along with the one I created in the first parts of this article to test this out. You can get the code from Microsoft's website at this [link](https://docs.microsoft.com/en-us/sql/relational-databases/graphs/sql-graph-sample?view=sql-server-2017).
<!-- more -->
Below is the additional R code required to generate the visualization. A couple of Tidyverse packages (tibble amd magrittr) and the DiagrammeR package and needed and all are available on the CRAN repository. Copy and paste the code below right after the last line of the part 3 code that ends with this:

DROP TABLE ##NodeEdgeXRef;
')

## New code

\# Read in distinct node names and IDs into a tibble and call the column nodes, and 
nodelist <- tibble(nodeName = union(nodes\_edges$FromName, nodes\_edges$ToName), 
                   nodeId = union(nodes\_edges$FromNode, nodes\_edges$ToNode))

# Create the graph object
i\_graph <-
  create\_graph()  %>%
    ## Get the nodelist tibble and define it as a table for the create\_graph function
     add\_nodes\_from\_table(
        table = nodelist,
        label\_col = nodeName) %>%

    ## Define the To and From edge names
            add\_edges\_from\_table(
            table = nodes\_edges,
            from\_col = FromNode,
            to\_col = ToNode,
              from\_to\_map = nodeId)  %>%

                    ## set the node fontsize to 5. Adjust larger or smaller depending on table name length   
                    set\_node\_attrs(
                       node\_attr = fontsize,
                       values = 5
                    ) %>% 

                    ## Set the node name text so it will display on the nodes  
                    set\_edge\_attrs(
                       edge\_attr = value,
                       values = nodes\_edges$EdgeName) %>%
                    
                    ## Set the edge fontsize to 5. Adjust larger or smaller depending on table name length
                     set\_edge\_attrs(
                        edge\_attr = fontsize,
                        values = 5) %>%
                    ## Set the edge name values between nodes. 
                     set\_edge\_attr\_to\_display(
                        edges = 1:(nrow(nodes\_edges)),
                        attr = value,
                        default = NA);

## Display the graph
render\_graph(i\_graph, layout = "nicely")

That's it! Save the script and run it, and you should see the graph I showed last time for the StudentClass demo database, or if you use the Microsoft sample, the visualization at the top of this page. I am continue to explore Graph Databases and will post more information as I develop it. I'm also happy to say that Microsoft has already announced additional features for graph databases in SQL Server 2019.