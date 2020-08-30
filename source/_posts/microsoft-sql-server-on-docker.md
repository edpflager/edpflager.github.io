---
title: Microsoft SQL Server on Docker (part 1)
tags:
  - How-to
  - howto
  - install
  - SQL Server
  - SysAdmin
  - technical
id: '3447'
categories:
  - - Blog
  - - Business Intelligence
  - - Docker
  - - Linux
comments: false
date: 2016-11-28 10:45:09
---

[![sql_on_docker](http://edpflager.com/wp-content/uploads/2016/11/sql_on_docker-300x229.jpg)](http://edpflager.com/?attachment_id=3463#main)After last week's announcement that Microsoft has released a Public Preview version of SQL Server that would run on Linux platforms (currently Ubuntu 16.04 and Red Hat Enterprise 7.2), I wondered if a Docker version was going to be made available as well. Checking out Microsoft's [SQL Server v.Next website](https://www.microsoft.com/en-us/sql-server/sql-server-vnext-including-Linux?wt.mc_id=AID533228_EML_4683749#resources), I quickly saw that **Lo and behold!** there was! As I stated previously, a good portion of my professional career has been spent working with Microsoft SQL Server and a lot of my spare time is spent experimenting with Linux and Open Source applications. The merging of the two is hugely exciting to me and the ability to run SQL Server in a Docker container is amazing! Previous Docker containers of Microsoft products have been a mixed bag of requirements. Some required you to run the Docker engine on a Windows platform in order for it to work, while others could be run on different Linux variants. Even the previously released version of SQL Server Express, the free version of SQL would only work on Windows hosted containers. But so far it seems with this version of SQL Server, Microsoft may be starting to explore the open source world possibilities. There are some caveats though:
<!-- more -->
*   So far, the Linux based version of SQL Server only has command line tools available. If you want to use the standard SQL Server Management Studio GUI tool, you'll need a Windows system to connect to your Linux SQL Server.
*   The Linux version of SQL Server does not support Active Directory authentication, a big part of the Microsoft security model. With AD, you can use the same user account across multiple machines, and don't need to login to each one, and your credentials are maintained in a central location.
*   The Linux version does not include Reporting Services or Analysis Services, two other components of the Microsoft BI Stack. While not essential for many organizations, these tools are included as part of a fully licensed version of SQL Server on a Windows platform.
*   And finally, there is a big issue of licensing. This Preview edition of SQL Server is only operational for a few months. Normal practice for Microsoft's preview versions to keep folks from running it in perpetuity. But what is Microsoft's license model for SQL Server running on Linux? By not requiring a Windows platform they are taking part of their revenue stream away, but without the other features of the BI stack is it a non-starter for many organizations?

All of this may change, and is likely to as development continues, but for now, lets look at how to get a SQL Server Docker container running. Connect to your Docker host, and create a folder where you'd like your database files to persist. This way, when your container shuts down for whatever reason, you will stay have you data files. In my case, I created it under my user folder:

mkdir mssql

Then enter the following command to start the container. You'll need to include a strong system administrator password as part of the command using the enviroment setting -e SA\_PASSWORD. The default port that SQL Server uses is 1433. If you want to change that, you can alter the -p option. Substitute the path to your data folder after the -v flag. For my case, the command to start my container looks something like this:

docker run -e ACCEPT\_EULA=Y -e SA\_PASSWORD=string1password -p 1433:1433 -v /home/me/mssql:/var/opt/mssql -d microsoft/mssql-server-linux

On  your host machine in your data folder, you'll now see several new folders created: data, etc, log, and secrets. The data folder containers the four databases that SQL Server needs to run: master, model, msdbdata and tempdb. The etc folder is probably empty. The log folder is where logs are written to by the SQL Server engine, and the secrets folder contains a file identifying your machine.

Next time I'll walk through installing the SQL Server command line tools and restoring a copy of AdventureWorks to your Docker container.