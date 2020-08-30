---
title: Debugging ETL with Error Output
tags:
  - ETL
  - How-to
  - kettle
  - Linux
  - PDI
id: '2208'
categories:
  - - Blog
  - - Business Intelligence
  - - Pentaho
comments: false
date: 2014-08-15 19:05:08
---

[![sys-error](http://edpflager.com/wp-content/uploads/2014/07/sys-error-300x124.png)](http://edpflager.com/wp-content/uploads/2014/07/sys-error.png)When developing new ETL flows, at an early stage you should include steps for error output so you can more easily locate and fix problems, especially when going between different data platforms. As an example, I have recently been working on data transformations that move records on a regular basis between PostgreSQL, Microsoft SQL Server and a DB2 mainframe system. All of these systems use similar data types, but not always the same ones and the method they use to handle data types may vary as well. Generally what I do is create an On Error step at the destination points in the transformations, and dump any bad records to a text file. This allows me to review if the error is affecting all of the records ( which would indicate a problem with the configuration) or only some of the records (which may indicate a problem with how the step is coded). And in some cases the error output indicates an issue with the source data that wasn't foreseen! The biggest benefit however is that you get to run the complete sample of records through your workflow to see how well it processes different values, rather than having it fail on the first error.
<!-- more -->
Here is an illustration of what I've stated above using Pentaho Kettle. Its a simple workflow with a table input and table output. Your ETL workflows are likely to be more involved then this, but for this discussion its sufficient.![errorout](http://edpflager.com/wp-content/uploads/2014/08/errorout-300x77.png) After the Table Output step, create another step that writes data to a text file. (You could use a table in your destination database just as easily.) From the Table Output step, drag a hop to the Text file output step. At this point a small menu will appear allowing  you to set this as the Main output  of step (the normal option) or Error Handling of step. Choose this option, and the hop looks like the one above, with the red line connecting the step steps. [![setup](http://edpflager.com/wp-content/uploads/2014/08/setup-300x67.png)](http://edpflager.com/wp-content/uploads/2014/08/setup.png) Now you are ready to test your ETL workflow. Execute the workflow and watch the Step Metric tab in the results panel. You can see how many records are moved to the destination and how many are written to the error handling step. If I see that most of the records process, I know I am on the right track, and if most or all of the records fail, then I have some serious issues I need to investigate.

* * *

  Pentaho is a trademark of PENTAHO, Inc.