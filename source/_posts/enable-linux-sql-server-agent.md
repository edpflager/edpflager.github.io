---
title: Enable Linux SQL Server Agent
tags:
  - How-to
  - howto
  - SQL Server
  - SysAdmin
  - technical
id: '3567'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2018-03-05 18:16:38
---

[![](http://edpflager.com/wp-content/uploads/2018/02/sql-server-icon.png)](http://edpflager.com/2017/09/23/photo-break-ambassador-bridge-windsor/3623-revision-v1/)Short tip this time around. If you are running SQL Server on Linux and connect to it from a Windows system with Management Studio (SSMS), the SQL Agent will  be off. If you right click the Agent in SSMS try to get the properties of it, you will receive a lengthy  error with this included:

SQL Server blocked access to procedure 'dbo.sp\_get\_sqlagent\_properties' of component 'Agent XPs' because this component is turned off as part of the security configuration for this server.

All good and normal. So how do you turn SQL Agent on?
<!-- more -->
Open a query window and execute this script:

```
sp_configure 
```

Finally, right click on SQL Agent in SSMS and choose Refresh from the menu. You should now be able to access it.

_**SQL Server is a product of Microsoft Corporation.**_