---
title: Shell Script to control services on Linux Mint (especially SQL Server)
tags:
  - How-to
  - howto
  - Mint
  - SQL Server
  - SysAdmin
  - technical
  - Ubuntu
id: '3692'
categories:
  - - Blog
  - - Linux
comments: false
date: 2018-02-28 04:01:44
---

[![](http://edpflager.com/wp-content/uploads/2018/02/shell.png)](http://edpflager.com/?attachment_id=3694#main)I've posted before about how I don't run certain services on my Linux system all the time, but rather only when I am working with them. For example, Docker, Pentaho applications, and several database servers like MySQL, MariaDB and now Microsoft SQL Server.  The reasons are simple: because I experiment with a variety of technologies, I don't want to dedicate resources unnecessarily and there may often be conflicts such as web server interfaces using the same ports. So to alleviate some of those problems I generally disable services and start them when necessary. To do that I usually create Bash scripts to start and stop the services, and save those in a named location in /opt that is associated with the server application. I would have two scripts, one to start and one to stop, but since I've had some time lately I've worked up a method to do this in one script. Below is a script I drafted to start and stop Microsoft SQL Server on Linux. I saved this to the /opt folder where the SQL Server components are installed and then created a Launcher shortcut to add it to my menu. Now I just select that option in the menu, and I see the current status of the server, and I can start it or shut it down as need be. Its heavily commented to provide information, so use it as a source for yourself if you are running on Debian based systems.
<!-- more -->
#! /bin/bash
# Previous line explicitly names which BASH shell to run. Some versions of Linux use a different 
# shell when running from GUI.

# Check the status of mssql-server service. Redirects output to /dev/null to suppress printing 
# results because we are only interested in the return code.
systemctl status mssql-server | grep 'Active: inactive (dead)' &> /dev/null

# Check return code. If result is zero then SQL Server Service is not running.
if \[ $? == 0 \]; then 
 echo "SQL Server is not active."
 
# Prompt use to start SQL Server. Loop through until user responses with a Yes or No. 
#   Result is stored in start variable.
    while true
    do
    read -p "Do you want to start it (Y/N)?" start

   # Case statements to check entered value
     case $start in
               # If y or Y entered, start service then exit.
               \[yY\]\* ) sudo systemctl enable mssql-server && sudo systemctl start mssql-server
                break ;;
 
               # If n or N entered, do nothing and exit.
               \[nN\]\* ) exit;;

               # If Y or N not entered, prompt for correct entries
               \* ) echo "Please enter Y or N";;

       # End case statement and loop again 
       esac
       done
# If return code from service check is not zero then SQL Server Service is running.
else 
 echo "SQL Server is active"

# Prompt use to stop SQL Server. Loop through until user responses with a Yes or No. 
#  Result is stored in start variable.
   while true
   do
   read -p "Do you want to stop it (Y/N)?" stop

   # Case statements to check entered value
   case $stop in
              # If y or Y entered, start service then exit.
              \[yY\]\* ) sudo systemctl stop mssql-server && sudo systemctl disable mssql-server
              break ;;

              # If n or N entered, do nothing and exit. 
              \[nN\]\* ) exit;;

              # If Y or N not entered, prompt for correct entries
                  \* ) echo "Please enter Y or N";;
 
              # End case statement and loop again 
              esac
              done

# End IF statement
fi

#Exit script
exit