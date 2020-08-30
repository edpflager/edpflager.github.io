---
title: Send Push Notifications from your Kettle jobs - Android
tags:
  - ETL
  - How-to
  - howto
  - kettle
  - PDI
  - technical
id: '2417'
categories:
  - - Blog
  - - Business Intelligence
  - - Pentaho
comments: false
date: 2014-09-10 18:52:59
---

[![notification](http://edpflager.com/wp-content/uploads/2014/09/notification-300x199.png)](http://edpflager.com/wp-content/uploads/2014/09/notification.png)When supporting ETL processes, one of the things you have to be conscious of is job failures. To get timely notification of those failures you can include email messages within your workflows that are generated when a failure occurs. Another way that is available, that many Pentaho Data Integration users may not be aware of, is the ability to send Push notifications out from your workflows. A developer named [Joel Latino](http://about.me/latinojoel) has supplied plugins to the Pentaho Marketplace that can be used to send notifications to Android and Apple iOS devices. In this post, I'll look at sending them to Android devices, and in a followup article I hope to cover sending to iOS using Joel's plugins.
<!-- more -->
###### Install the Marketplace plugin

There are a couple of setup steps that need to be performed to enable Push Notifications in PDI. First up is adding the plugin to the PDI environment. It will need to be added to all systems that PDI is installed on.

1.  Start the PDI graphical user interfacer (Spoon), click on Help in the menu bar, and then Marketplace in the drop down list.
2.  [![marketplace](http://edpflager.com/wp-content/uploads/2014/09/marketplace-300x239.png)](http://edpflager.com/wp-content/uploads/2014/09/marketplace.png)A new window will appear showing the available plugins in the Marketplace.
3.  Locate the Android Push Notifications plugin in the list and toggle the entry to see the detail. At the bottom of the information there should be a Install this Plugin button. Click on it and PDI will download the necessary files and add them to the environment. Restart PDI to make it active.

###### Install the Android app

1.  On the Android device that will receive the notifications, access the Google Play store, and download and install the PDI Manager app. Once its installed start it up.
2.  Access the menu for the application using the menu button on the device.
3.  From the menu popup, choose Registration ID, and a new screen will appear with your registration ID. Click the SEND button to choose a method to transmit the ID to the PDI system.

###### Create a transformation

For this portion I will be using a modified version of Joe Latino's sample transformation to illustrate the .

1.  Create a new transformation in PDI.
2.  On the Design tab in the left panel, locate the Input node, toggle it to open it and drag a Generate Rows object to the work space.
3.  From the Scripting node, drag a Modified Java Script object to the work space.
4.  Finally from the Utility node, drag the Android Push Notification object to the work space.
5.  Create two hops to link the three objects together and it should resemble this :[![transformflow](http://edpflager.com/wp-content/uploads/2014/09/transformflow-300x35.png)](http://edpflager.com/wp-content/uploads/2014/09/transformflow.png)
6.  Open the Generate Rows object, and at the top of the window set the Limit value to 1.
7.  In the grid at the bottom, add four string fields: registrationid, status, data and project.
8.  In the Value column for the registration field, enter the  registration ID that was generated from the Android app.
9.  For the status field, enter a value of Error or Success. Those are the only two accepted options.
10.  For data, enter a status message that corresponds to the notification, providing more detail.
11.  Finally for the project field, enter an appropriate value, such as the workflow name or the ETL job that called the transform. The results should be similar to this:[![GenerateRows](http://edpflager.com/wp-content/uploads/2014/09/GenerateRows-300x153.png)](http://edpflager.com/wp-content/uploads/2014/09/GenerateRows.png)
12.  Click OK to exit, and double click on the Script object to edit it.
13.  In the Script 1 area of the screen, paste this code in to get the current date and time: var simpleDateFormat = new java.text.SimpleDateFormat("yyyy/MM/dd HH:mm:ss.SSS"); var date = simpleDateFormat.format(java.util.CalendarTodaysDateTime.getInstance().getTime());
14.  At the bottom of the window, enter a field name - "date" with a data type of string. The results should look like this: [![javascript](http://edpflager.com/wp-content/uploads/2014/09/javascript-300x175.png)](http://edpflager.com/wp-content/uploads/2014/09/javascript.png)
15.  Click OK to exit. Double click the Push Notification object to configure it.
16.  In the Main Options tab, drop the Registration ID list down and choose registrationid from the values.
17.  At the bottom of the window, click the Get Fields button, and five lines will be pulled in. Delete the line called registrationid leaving the four other values.![maintab](http://edpflager.com/wp-content/uploads/2014/09/maintab-300x270.png)
18.  Switch to the Properties tab and enter Joel Latino's API key (AIzaSyAh7Nf-N7bE4xIwsVb7nk4mmls\_yEQwZQA) in the appropriate spot. (To use a different Google Cloud Messaging key, it should be possible to alter Joe's source code and change it to point to a new server key. I have not tried it though).![propertiestab](http://edpflager.com/wp-content/uploads/2014/09/propertiestab-300x269.png)
19.  Leave the other values as they are. Click OK to return to your workflow.

######  Test the transformation

1.  Execute the transformation and check the Android device and a notification should appear almost immediately.
2.  Open the PDI Manager to get more detail on the notification, and to clear it.

###### Things to try out

At this point the push notification works. There are some additional functionality I would like to experiment with as I have time:

*   For success notifications, in the PDI manager there is only an option to delete the notification. When an error notification is sent, there are two options: Delete or Resolved.  I am interested in working on implementing code to acknowledge the Resolved option.
*   The Generate Rows step is used normally for testing. I would like to implement code to define the fields using a different object.
*   I am interested in creating a transformation that is reusable, which would require passing a variable to the transformation indicating the job name.
*   Finally, I'm not fond of using JavaScript, and would like to replace the code that generates the time/date field with using a System Information step to pull that information.

Thanks to Joel Latino for his plugin! Pentaho is a trademark of Pentaho LLC.