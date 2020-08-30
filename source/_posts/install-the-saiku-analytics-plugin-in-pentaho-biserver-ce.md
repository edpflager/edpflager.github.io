---
title: Install the Saiku Analytics plugin in Pentaho BIServer CE
tags:
  - Big Data
  - ETL
  - guides
  - How-to
  - howto
  - SysAdmin
  - technical
id: '3322'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
  - - Pentaho
comments: false
date: 2016-06-05 05:52:15
---

[![meterorite](http://edpflager.com/wp-content/uploads/2016/06/meterorite-300x180.jpg)](http://edpflager.com/?attachment_id=3324#main) I've been working with Mondrian and Pentaho's Schema Workbench lately and attempted to add Meteorite Consulting's Saiku Analytic plugin to my installation of Pentaho BI Server community edition, to process some MDX queries. MDX is a query language similar to SQL that is used for processing database cubes. Mondrian is a OLAP engine that implements the MDX language and is incorporated into the Saiku Analytic software. It differs from other OLAP engines in that the cubes are built on the fly as the query processes, rather than having the cube data stored on a server. For simpler cubes, the trade off between a slightly slower build time and disk space is negligible. Here is the process I followed to get Saiku enabled in my BI Server:
<!-- more -->
1.  Access the [Pentaho Marketplace](http://www.pentaho.com/marketplace/) and search for the Saiku Analytic plugin.
2.  Click on the plugin from the results window and in the popup window, click the **DOWNLOAD PLUGIN** button.
3.  After it completes downloading, extract the ZIP file. It should generate its own folder called "saiku".
4.  Move the "saiku" folder to the **biserver-ce/pentaho-solutions/system** folder. In my case that was under **/opt/pentaho**. Your version may be different.
5.  Access the [Meteorite Consulting's license website](http://licensing.meteorite.bi/)  and signup for a new account (its free). For company, I just used my address.
6.  Once you have validated your account, login to the system, and click the **CREATE NEW LICENSE** button.
7.  On the new license page, enter the hostname of the machine that biserver runs on. Enter one for the the maximum number of users (I believe it is irrelevant for the Community version). If you are not comfortable with that, enter how many actual users you think will be using it. Set the license type to **COMMUNITY EDITION**. Your user name and company(address) info will already be filled in.
8.  Click the **SAVE** button, and the page will update with a link to **Download License**. Click it, and a license file will download to your system, as the hostname you specified and the extension of ".lic".
9.  Rename the file to "license.lic" and then copy it to the "saiku" folder that you moved under bi-server/pentaho/system previously.
10.  Restart your biserver.
11.  Login to your biserver as a user with analytics permissions.
12.  Click FILE -> NEW and you should have an option for SAIKU ANALYTICS.
13.  Click it and if all was done correctly, the SAIKU ANALYTICS page will load.