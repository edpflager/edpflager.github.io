---
title: ApacheDS (LDAP) - Part 4
tags:
  - cookbook
  - guides
  - How-to
  - howto
  - install
  - LDAP
  - Mint
  - SysAdmin
  - technical
id: '3127'
categories:
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2015-12-03 12:57:56
---

[![users](http://edpflager.com/wp-content/uploads/2015/12/users-300x213.png)](http://edpflager.com/wp-content/uploads/2015/12/users.png)This is part 4 (the last part) of my series on using Apache Directory Server (ApacheDS), an open source LDAP implementation. Parts [1](http://edpflager.com/?p=3029)\-[2](http://edpflager.com/?p=3055) covered getting ApacheDS installed, as well as connecting the configuration application to it to make changes to the structure of the directory. In [part 3](http://edpflager.com/?p=3081), I walked through adding a new Organizational Unit from scratch to your LDAP tree. In this last part I'll cover adding a new user, using an existing user object as a template.

##### ADD A NEW USER

If you have been following these tutorials in order, you should have an DIT tree with an OU called Employees under your DC entry. Right click on the Employees OU you just created and chose New -> New Entry from the menu that appears. You'll be prompted to select : "Create entry from scratch" or "Use existing entry as a template". Choose the option to Use an Existing entry as a template. Then click the BROWSE button on the right.
<!-- more -->
[![Browse for Object](http://edpflager.com/wp-content/uploads/2015/12/Browse-for-Object-252x300.png)](http://edpflager.com/wp-content/uploads/2015/12/Browse-for-Object.png)A window titled **Select DN** will open. Toggle the arrows until you find the entry labeled "uid=admin". Click it to highlight it, and then click the OK button at the bottom of the window. [![Browse for Object](http://edpflager.com/wp-content/uploads/2015/12/Browse-for-Object-252x300.png)](http://edpflager.com/wp-content/uploads/2015/12/Browse-for-Object.png) The window will close, and you will be returned to the New Entry window. The result should look like this: [![entry from existing](http://edpflager.com/wp-content/uploads/2015/12/entry-from-existing-278x300.png)](http://edpflager.com/wp-content/uploads/2015/12/entry-from-existing.png) Click the NEXT button to continue. The window will update and show the available Object Classes on the left. There will be several already entered on the right. Click the tlsKeyInfo item on the right to select it, and click the REMOVE button. You should be left with these four items:[![orgperson](http://edpflager.com/wp-content/uploads/2015/11/orgperson-275x300.png)](http://edpflager.com/wp-content/uploads/2015/11/orgperson.png) Click the NEXT button to continue. In the Distinguished Name window, change the uid= to the name of your new object. In my case that would be "biadmin". Now add one attribute: cn = "biadmin".[![entry_disting](http://edpflager.com/wp-content/uploads/2015/12/entry_disting-279x300.png)](http://edpflager.com/wp-content/uploads/2015/12/entry_disting.png) If the DN Preview is populated successfully click NEXT to advance to the next screen. On the Attributes page you'll see an error message indicating that several attributes are not allowed or are not filled in with a valid entry. [![biadmin_attributes_remove](http://edpflager.com/wp-content/uploads/2015/12/biadmin_attributes_remove-246x300.png)](http://edpflager.com/wp-content/uploads/2015/12/biadmin_attributes_remove.png) Delete these values by highlighting them with a click and then clicking the red X that will appear at the top right:

*   the second CN with a value of "system administrator"
*   displayName
*   keyAlgorithm
*   privateKey,
*   privateKeyFormat,
*   publicKey,
*   publicKeyFormat
*   userCertificate.

Click OK when prompted to see if you are sure.  Click the Value field next to SN and change the text to "bi administrator". When completed, you should be left with the values below.[![biadmin_attributes_endresult](http://edpflager.com/wp-content/uploads/2015/12/biadmin_attributes_endresult-249x300.png)](http://edpflager.com/wp-content/uploads/2015/12/biadmin_attributes_endresult.png)Now we need to enter a user password. Double click the value field next to userPassword that currently says Empty Password and a Password Editor window will appear. Click the New Password tab to work with it. Enter a password for the user in the Enter New Password field, and then again in the Confirm New Password field.The password is stored with a HASH method, the default being "plaintext". If you'd like to use a different Hash Method to encrypt your password, choose the option from the drop down list. You can check the box labeled **Show new password details** and the mask on the password fields will be replaced by the values you entered.[![password_editor_change_filled](http://edpflager.com/wp-content/uploads/2015/12/password_editor_change_filled-300x227.png)](http://edpflager.com/wp-content/uploads/2015/12/password_editor_change_filled.png) Click the OK button and the window will close and return you to the New Entry - Attributes window. Click OK again, and after a few seconds, your new user will be created! [![biadmin_added](http://edpflager.com/wp-content/uploads/2015/12/biadmin_added-300x147.png)](http://edpflager.com/wp-content/uploads/2015/12/biadmin_added.png)   This is the conclusion of my series on setting up Apache Directory Server.