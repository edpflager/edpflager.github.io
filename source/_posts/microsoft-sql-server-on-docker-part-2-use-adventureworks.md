---
title: Microsoft SQL Server on Docker (part 2) - Use AdventureWorks
tags:
  - cookbook
  - guides
  - How-to
  - howto
  - install
  - SQL Server
  - SysAdmin
id: '3451'
categories:
  - - Blog
  - - Business Intelligence
  - - Docker
  - - Linux
comments: false
date: 2016-11-30 18:58:33
---

[![sql_on_docker](http://edpflager.com/wp-content/uploads/2016/11/sql_on_docker-300x229.jpg)](http://edpflager.com/?attachment_id=3463#main)In [part one of this series](http://edpflager.com/?p=3447), I provided some info on Microsoft's implementation of Sql Server on Docker and provided a method to have your SQL Server databases saved on your Docker host system so that they would remain persistent if the SQL Server container was shutdown. This time around, we'll look at how to restore Microsoft's sample database AdventureWorks to your SQL Server container.

##### DOWNLOAD ADVENTUREWORKS

If you followed my instructions from part 1 of this article you have a SQL Server container up and running with a data folder on your Docker host. Within that folder, you have several subfolders that SQL Server uses. You need to download a copy of AdventureWorks and save it in that folder. There are multiple versions available, as Microsoft provides a new version with every upgrade to SQL Server. The 2014 version should work fine for this purpose (If you download the 2016CTP version the restore command below will not work because that version includes FILESTREAM as part of the backup. An addition MOVE command needs to be supplied to include that in the RESTORE). Go to this [website](https://msftdbprodsamples.codeplex.com/releases/view/125550), and download Adventure Works 2014 Full Database Backup.zip. I renamed the file to just AdventureWorks.zip to make it easier to transfer to my Docker host system and used scp to transfer it:
<!-- more -->
scp AdventureWorks.zip <username>@<Docker Host IP>:~/AdventureWorks.zip

Now unzip the file to the shared data folder that was created previously. SUDO is required because the folder is protected:

sudo unzip AdventureWorks.zip -d ~/mssql/data

 

##### RESTORE ADVENTUREWORKS

Before we can restore a database, we need a Linux or Mac system with the Microsoft SQL Server command line tools installed or a Windows machine with the SQL Server Management Studio installed. The instructions below are just for using the command line tool however. Attempting to install the tools on the SQL Server container currently fails with several unmet dependency issues, and installing the tools in an Ubuntu container doesn't appear to work either, resulting in an error when attempting to access the sqlcmd tool. So on another Linux machine, execute the following commands to install the SQL Server command line tools. If you Docker host machine is running Ubuntu you can install the tools there with these instructions.

Refresh the repository information in your container with:

apt-get update

Then install CURL if you don't have it:

apt-get install curl

Fetch the Microsoft GPG registry key and install it:

curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -

Register the Microsoft repository for Ubuntu (since that is what the container is built on)

curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list | tee /etc/apt/sources.list.d/msprod.list

Install the https transport component for Ubuntu:

```
apt-get install apt-transport-https
```

And update your repository cache again:

apt-get update

Finally, install the SQL Server command line tools:

```
apt-get install mssql-tools
```

During the installation you will prompted to accept the license terms twice. Enter Yes both times if you accept and want to proceed. Once the installation completes, access SQLCMD and connect to your SQL Server machine:

sqlcmd -S <SQL Server IP> -U SA -P <password>

The display should respond with:  1>

You can then restore the AdventureWorks database with this command, entering each line with a RETURN at the end:

RESTORE DATABASE AdventureWorks FROM DISK = 'C:varoptmssqldataAdventureWorks.BAK'
WITH MOVE 'AdventureWorks\_Data' TO 'C:varoptmssqldataAdventureWorks2014\_Data.mdf',
MOVE 'AdventureWorks\_Log2014' TO 'C:varoptmssqldataAdventureWorks\_Log.ldf'
GO

If you typed everything correctly you should see a number of rows displayed as the database is restored and after a second or two a success message. [![resotore_start](http://edpflager.com/wp-content/uploads/2016/11/resotore_start-1024x484.png)](http://edpflager.com/?attachment_id=3467#main)Switch to the new database with:

USE AdventureWorks
GO

The response should be Changed Database context to 'AdventureWorks'! You have restored AdventureWorks, and can experiment with it!