---
title: Use GMail with Pentaho BI-Server Community
tags:
  - cookbook
  - guides
  - How-to
  - howto
  - SysAdmin
  - technical
id: '3157'
categories:
  - - Blog
  - - Business Intelligence
  - - Pentaho
comments: false
date: 2015-12-08 15:15:51
---

Email servers are fairly common in a lot of organizations, but many smaller companies elect to use outside hosting for their email. There are myriad reasons for doing so, and the choice obviously makes sense, otherwise they wouldn't do it. If your organization is using Google Gmailfor its email, you can set up Pentaho's BI Server to use it as your email server. In this brief article, I'll walk you through the settings you'll need to use to make it work. ![Home menu](http://edpflager.com/wp-content/uploads/2015/12/Home-menu-195x300.png)To access the email settings in the Pentaho User Console, login to your BI-Server website with an Administrator account. Click on the large HOME menu item, and at the bottom of the menu that appears, click on Administration. When the Administration screen appears, click on the Email Server option in the menu on the left. The screen will update with the Email Server settings fields. Below are instructions for what information to use to populate the fields depending on the hosting service you are using.
<!-- more -->
GMAIL [![GMail](http://edpflager.com/wp-content/uploads/2015/12/GMail1-143x300.png)](http://edpflager.com/wp-content/uploads/2015/12/GMail1.png) For Gmail, some of the settings are non-standard, but they are well documented. The settings are shown in the screen shot to the left. Just a few notes:

*   For port use 587.
*   From the Server Type drop down, choose SMTPS.
*   Finally at the bottom of the screen check both boxes next to Use Start TLS and Use SSL.

Click the Test Email Configuration after you are complete. If everything is correct, after a few seconds you'll see a message that the test was successful and in the email account you specified, you'll have a test email with the subject of Diagnostic Verification Email.