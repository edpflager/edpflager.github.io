---
title: Extract PDF data with Tabula
tags:
  - centos
  - ETL
  - How-to
  - howto
  - install
  - Mac
  - technical
id: '2967'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2015-09-28 19:34:28
---

[![tabula](http://edpflager.com/wp-content/uploads/2015/09/tabula-300x176.png)](http://edpflager.com/wp-content/uploads/2015/09/tabula.png)Adobe's PDF file format is a wonderful tool, allowing users on disparate operating systems to share documents easily. Because Adobe made the file format an open-standard in 2008, applications to create and read PDF files readers can be found on pretty much every operating system you can think of - Linux distros, Windows, Mac OS X and BSD,  just to name a few, from no cost to several hundred dollars. And in most cases, the original document, if not identical to the PDF, is close enough to identical to make the differences irrelevant. In my work as a BI developer, occasionally I have to extract data from PDF documents and get it into a database. While reading the file is no problem, getting the data out in a usable format where I am not having to retype or reformat the output excessively is often times not so easy. Luckily I have come across an open-source tool, called [Tabula](http://tabula.technology/),  that makes extracting data from a PDF much easier. It doesn't work for every PDF, only on text-based PDFs. That means reports and data sets that were exported to a PDF file, rather than documents that were scanned into a computer and saved as a PDF file. (The latter tends to be image type files rather than text based documents.)
<!-- more -->
Tabula is available for Windows, Mac OS X and Linux and other operating systems. Generally it appears that if the OS has a JAVA 6 or 7 runtime engine,  then you should be able to use Tabula on it. The implementation is interesting in that it launches a localhost web server as an interface to the application, but I haven't seen any security problems with using it.

1.  For illustration purposes, download this one page PDF from DATA.GOV: [Los Angeles Debt Ratings](https://catalog.data.gov/dataset/city-of-los-angeles-debt-ratings-df4b6) and then download the version of Tabula appropriate for your platform.(Scroll through the Tabula notes window on the right to find the link for the Linux/Other version).
2.  Extract the file to a location that's easily accessible. Windows and Mac versions will have a dedicated application in the output folder and the Linux/Other version will have a JAR file.
3.  To execute the JAR, open a terminal prompt and change directory to where Tabula was extracted. Enter this command to start the Tabula webserver: java -Dfile.encoding=utf-8 -Xms256M -Xmx1024M -jar tabula.jar
4.  Several lines of text will indicate the web server is starting and once its ready, your default web browser will open, pointing to http://127.0.0.1:8080/ and prompting you to import a PDF.![TabulaImport](http://edpflager.com/wp-content/uploads/2015/09/Screenshot-from-2015-09-28-212858-300x99.png)
5.  Click the Browse button and navigate to where you saved the Debt Ratings file, and then click the IMPORT button. The file will be imported, and a visual representation will be displayed once its complete. Click and drag around the data you want to import.![Selection2](http://edpflager.com/wp-content/uploads/2015/09/Selection2-300x169.png)
6.  Once you have the data selected, click the Preview and Export Extracted Data button. Tabula will process your selection and then display a grid of the data you selected.![Output](http://edpflager.com/wp-content/uploads/2015/09/Output-300x163.png)
7.  Select the file format you would like to produce from the drop down list at the top of the screen, and then click the Export button. Tabula will generate a file in our browser's download location after a few seconds of processing.
8.  If you'd prefer to copy the information to your system's clipboard, click the Copy to Clipboard button instead, and then paste the data into whatever application you would like. Below is a screen shot from LibreOffice Calc where I have pasted the data from the sample PDF.[![LibreOfficePaste](http://edpflager.com/wp-content/uploads/2015/09/LibreOfficePaste-e1443491875487-300x188.png)](http://edpflager.com/wp-content/uploads/2015/09/LibreOfficePaste-e1443491875487.png)
9.  To shut down Tabula, switch back to the Terminal window and enter Ctrl-C to exit the JAVA application, and then Exit the terminal window.Adobe logo clipart from http://www.wcpga.com/