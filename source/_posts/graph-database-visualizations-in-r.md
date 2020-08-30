---
title: Graph Database Visualizations in R
tags:
  - ggplot2
  - graph database
  - howto
  - SQL Server
id: '4446'
categories:
  - - Big Data
  - - Business Intelligence
  - - Linux
  - - Misc
  - - R
comments: false
date: 2019-03-01 03:52:39
---

[![](http://edpflager.com/wp-content/uploads/2019/02/ClevelandCarriers.png)](http://edpflager.com/wp-content/uploads/2019/02/ClevelandCarriers.png)Its been a couple of weeks since my last post, and I've been experimenting more with graph databases. One of the more common graph database examples explores the relationship between air traffic carriers, and their routes. The nodes are the various cities that carriers fly between, routes are the edges connecting the nodes, and the number of carriers or the flights per day servicing those routes can be a value for the edge. For my data exploration, I started with the latest data from the [Bureau of Transportation Statistics website](https://www.bts.gov/), to get a data set of US airports, and the [latest breakdown of US air carriers](https://www.transtats.bts.gov/Tables.asp?DB_ID=110&DB_Name=Air%20Carrier%20Statistics%20%28Form%2041%20Traffic%29-%20%20U.S.%20Carriers&DB_Short_Name=Air%20Carriers). I uploaded the raw data to a SQL Server to create tables - a set of Graph database tables and a set of normal tables. Working with an assumption that a carrier who had a route out of city has a corresponding route into a city, I created queries to narrow down the returned data to only routes from Cleveland-Hopkins International and Detroit Wayne County airports. (As an aside, population for the Cleveland Metro area is about 2.0 million people, and for Detroit 4.3 million which looks to play into the results.
<!-- more -->
My first stab at visualizing this data was similar to what I did previously with DiagrammeR.  I loaded  the data from SQL Server into two data frames and added edges and nodes to a graph object. I did encounter some issues, since I am running SQL Server 2017 for Linux and R Studio on a laptop with only 4GB or RAM, where if I attempted to graph more that 30 nodes, my code would error out. Limiting the output to routes with more than 2 carriers allowed me to generate this visualization: [![](http://edpflager.com/wp-content/uploads/2019/03/DiagrammerAirline-1024x619.png)](http://edpflager.com/wp-content/uploads/2019/03/DiagrammerAirline.png) The center node is the Cleveland airport with the US Department of Transportation's three letter abbreviation. Surrounding it is a dandelion effect showing the various nodes that have a direct connection. Those nodes are also labeled with the destinations three letter abbreviation.

### ISSUES WITH THIS VISUALIZATION

I wasn't happy with visualization because it has a number of issues. First the abbreviations are difficult to decipher unless you are familiar with them. JFK and LGA are John Kennedy and LaGuardia airports in New York respectively, DTW is Detroit, but after that I have to look them up. Second, the placement of the airport nodes is somewhat random. If you run the code multiple times, it generates different placements. Third, there is no designation for routes with more traffic. Looking at the chart at  the top, you can see that there are eight carriers running between Cleveland and Chicago (ORD), but the edge line is no different  that the one to Raleigh/Durham which has three carriers servicing it. While I could have added a label or tool tip to the edge line to indicate the route count, and the same to the destination nodes, I wanted more geographic information as well. Unfortunately, DiagrammeR does not supply that  type of graph visualization. In the interest of completeness, here is the code to generate the dandelion.

library c(odbc)
library(DiagrammeR)
library(dplyr)
library(maps)

## Connect to the Database server
con <- dbConnect(odbc::odbc(), 
                 .connection\_string = "Driver={ODBC Driver 17 for SQL Server};Server=localhost;Database=AirTravel;Uid=sa;Pwd=\*\*\*\*\*\*\*\*;")

## Union of Cleveland airport information so its included in Airport list.
###### ADD in the ID code from the dataset and join on that. 
airports <- 
    dbGetQuery(con, 'SET nocount on;
                     SELECT ait.AIRPORT\_ID, 
                            LEFT(ait.AIRPORT,3) as AIRPORT\_CODE, 
                            ait.AIRPORT\_ID as DESTINATION\_ID,
                            ait.DISPLAY\_AIRPORT\_NAME, 
                            ait.DISPLAY\_AIRPORT\_CITY\_NAME\_FULL,
                            ait.LATITUDE,
                            ait.LONGITUDE,
                            ait.AIRPORT\_STATE\_NAME
                     FROM Routes rt
                     JOIN Airports ai ON rt.$from\_id = ai.$node\_id
                     JOIN Airports ait ON rt.$to\_id = ait.$node\_id
                     WHERE ai.AIRPORT\_ID = 11042 and rt.CarrierCount > 2
                     UNION ALL 
                     SELECT top 1 ai2.AIRPORT\_ID,
                            LEFT(ai2.AIRPORT,3) as AIRPORT\_CODE ,
                            ai2.AIRPORT\_ID as DESTINATION\_ID,
                            ai2.DISPLAY\_AIRPORT\_NAME, 
                            ai2.DISPLAY\_AIRPORT\_CITY\_NAME\_FULL,
                            ai2.LATITUDE,
                            ai2.LONGITUDE,
                            ai2.AIRPORT\_STATE\_NAME
                       FROM Routes rt2
                       JOIN Airports ai2 ON rt2.$from\_id = ai2.$node\_id
                      WHERE ai2.AIRPORT\_ID = 11042;')

###### ADD IN THE FROM AND TO AIRPORT ID CODES and JOIN ON THOSE.
## Get Routes edge data 
routes <-
     dbGetQuery(con, 'SET nocount on;
                      SELECT TOP 30 ait.AIRPORT\_ID as TO\_COL,
                             ai.AIRPORT\_ID as FROM\_COL,
                             CarrierCount
                      FROM Routes rt
                      JOIN Airports ai ON rt.$from\_id = ai.$node\_id
                      JOIN Airports ait ON rt.$to\_id = ait.$node\_id
                      WHERE ai.AIRPORT\_ID = 11042 and rt.CarrierCount > 2;')

graph <-
     create\_graph() %>%
      add\_nodes\_from\_table( table = airports, 
               label\_col = AIRPORT\_CODE) %>%
                    add\_edges\_from\_table(
                       table = routes, 
                       from\_col = FROM\_COL, 
                       to\_col= TO\_COL,
                       from\_to\_map = AIRPORT\_ID)

render\_graph(graph, layout = "nicely")   

### MAPS visualization

[![](http://edpflager.com/wp-content/uploads/2019/03/CLERoutes-1024x620.png)](http://edpflager.com/wp-content/uploads/2019/03/CLERoutes.png)   I like this visual much better than the dandelion. It shows the location of the origin node relative to the destination nodes, and by coloring only the states where there are destinations, you can see that there are clusters where you can't get a direct flight from Cleveland. Unlike the first visual, this one remains static so whenever you run it with the same dataset, you will get the same visual. There are still some things I don't like with this visual, though. The destination nodes don't indicate the city, and the lines don't indicate the number of carriers servicing those routes. I did attempt to incorporate the plotly package into this graph to provide tooltips, but it does not work with the geom\_curve aesthetic in ggplot2. I'd also like to use parameters to make the code reusable. You would be prompted to select a city and then the visual would update from that the location. Although this is not complete, the code as it currently looks is below. I expect to be working longer hours on a work project so I wanted to get this out before too much time has passed between posts.

library(ggplot2)
library(maps)
library(odbc)
library(dplyr)

## Connect to the Database server 
con <- dbConnect(odbc::odbc(), 
           .connection\_string = "Driver={ODBC Driver 17 for SQL Server};Server=localhost;Database=AirTravel;Uid=sa;Pwd=\*\*\*\*\*\*\*\*;")

## Get Airport node data that are destinations from selected airport - could retrieve more data as necessary.
## Union of Cleveland airport information so its included in Airport list.

###### This is the destination data set. 
airports <- 
     dbGetQuery(con, 'SET nocount on;
                      SELECT DISTANCE, 
                             DEST\_CITY\_NAME, 
                             aid.DISPLAY\_AIRPORT\_NAME as DEST\_AIRPORT, 
                             aid.LATITUDE as DEST\_LATITUDE, 
                             aid.LONGITUDE as DEST\_LONGITUDE,
                             aid.\[Age (in years)\] as DEST\_AIRPORT\_AGE,
                             aid.AIRPORT\_STATE\_NAME,
                             aio.LATITUDE as ORIG\_LATITUDE, 
                             aio.LONGITUDE as ORIG\_LONGITUDE, 
                             aio.\[Age (in years)\] as ORIG\_AIRPORT\_AGE, 
                             sum(CarrierCount) as TotalRoute
                        FROM Routes2018 rte 
                        JOIN USAirports aid ON rte.DEST\_AIRPORT\_ID = aid.AIRPORT\_ID 
                        JOIN USAirports aio ON rte.ORIGIN\_AIRPORT\_ID = aio.AIRPORT\_ID
                       WHERE ORIGIN\_AIRPORT\_ID = 11042
                         AND aid.AIRPORT\_IS\_LATEST = 1
                         AND aio.AIRPORT\_IS\_LATEST = 1
                         AND class = 'F'
                         AND aid.AIRPORT\_STATE\_NAME NOT IN ('Hawaii', 'Puerto Rico', 'Alaska')
                    GROUP BY DISTANCE, DEST\_CITY\_NAME, aid.DISPLAY\_AIRPORT\_NAME, aid.LATITUDE, 
                         aid.LONGITUDE, aid.\[Age (in years)\], aid.AIRPORT\_STATE\_NAME, 
                         aio.LATITUDE, aio.LONGITUDE, aio.\[Age (in years)\];')

###### This is the origin data set. Use a parameter to generate this?
orig\_airport <- 
     dbGetQuery(con, 'SET nocount on;
                      SELECT LATITUDE as ORIG\_LATITUDE, 
                             LONGITUDE as ORIG\_LONGITUDE, 
                             \[Age (in years)\] as ORIG\_AIRPORT\_AGE
                        FROM USAirports 
                       WHERE AIRPORT\_ID = 11042
                         AND AIRPORT\_IS\_LATEST = 1;')
## Call the US States map
usa <- map\_data("state")

## Convert State names into a character vector. Have to convert names to lower case.
state\_names <- unique(airports$AIRPORT\_STATE\_NAME) %>% 
     lapply(tolower)

## Create a seperate data frame of destination states. This allows ggplot2 and maps to color them
destinations <- subset(usa, region %in% state\_names)

## Generate the visualization
ggplot(data = destinations) + 
       geom\_polygon(aes(x = long, y = lat, group = group), fill = "lightsteelblue", color = "black") + 
       coord\_fixed(1.3) + geom\_point(data = airports, aes(x= DEST\_LONGITUDE, y=DEST\_LATITUDE), shape = 10, color = "midnightblue", size = 2) +
       geom\_polygon(data = usa, aes(x=long, y = lat, group = group), fill = NA, color = "black") + 
       geom\_curve(data=airports, aes(x=ORIG\_LONGITUDE, y=ORIG\_LATITUDE, xend=DEST\_LONGITUDE, yend=DEST\_LATITUDE), curvature = 0.2, color="red") + 
       geom\_point(data=orig\_airport, aes(x=ORIG\_LONGITUDE, y=ORIG\_LATITUDE), shape = 8, color="red", size = 5)

  That's all I have for now. As I spend some more time on this, I will post a followup.