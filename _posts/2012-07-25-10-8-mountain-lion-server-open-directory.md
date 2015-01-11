---
title: '10.8 Mountain Lion Server: Open Directory'
author: Justin Rummel
layout: post
permalink: /10-8-mountain-lion-server-open-directory/
shorturl:
  - http://j.mp/MGHHKU
categories:
  - Apple
  - Mt Lion
  - OSXS
comments: true
---
Overview
--------
The concepts for installing Open Directory is exactly the same as previous versions of OS X Server. Select whichever you want (Master or Replica) and walk through the assistant to get your ODM/ODR running. Screenshots of this process are below with quick comments. The glaring item is that the true functionality of Open Directory is now LDAP, Kerberos, and PasswordServer; and nothing more. Workgroup Manager used to store its values within the ODM’s Apple LDAP schema, but WGM does not exist anymore (that’s what you get for writing before things are released… [WGM does exist!][DL1567]). The transition to using Configuration Profiles is now complete from Apple’s point of view, which is just like the transition to do everything via Server.app.

[DL1567]: http://support.apple.com/kb/DL1567

Things that I need to test;

*   Will Mt Lion clients honor MCX values that are configured by a ODM that is not running Mt Lion OS X Server?
*   What happens to your MCX values if you upgrade from Lion OS X Server?
*   are there any issues running MT Lion server with older versions of OS X Server (10.7, 10.6, 10.5)?

For that last bullet, it is always best practices to use the same version of OS X Server throughout your environment. The only exception would be if a server is bound to your Directory for Authentication only (thus not functioning as a ODM or ODR).

I’ll do my best to work on the first two bullets, PLUS AD integration for future posts.

The on thing that is missing from the GUI is a backup and Restore process. 

### Open Directory Master Setup

![mtl-ODM1][mtl-ODM1]
(Click the “On” button on the top right hand side. You should be used to this as everything uses this method to enable services.)

![mtl-ODM2][mtl-ODM2]
(Creating a ODM first.)

![mtl-ODM3][mtl-ODM3]
(The usual “diradmin” username and password.)

![mtl-ODM4][mtl-ODM4]
(This is for your SSL Certificates.)

![mtl-ODM5][mtl-ODM5]
(Verify information and click on the “Set Up” button.)

![mtl-ODM6][mtl-ODM6]
(Configuring your new ODM.)

![mtl-ODM7][mtl-ODM7]
(View of ODM server now complete.)

![mtl-ODM8][mtl-ODM8]
(Select your ODM Server, and click on the gear icon to set your Global Password Policy.)

![mtl-ODM9][mtl-ODM9]
(Locals are also available in Mt Lion OS X Server.)

![mtl-ODM10][mtl-ODM10]
(View of the default Locale configuration.)

Open Directory Replica Setup
----------------------------
![mtl-ODM11][mtl-ODM11]
(When you want to create a ODR, select the second option and click on “Next”.)

![mtl-ODM12][mtl-ODM12]
(Provide the ODM’s FQDN, diradmin username and password, then click on “Next”.)

![mtl-ODM13][mtl-ODM13]
(If you receive any error messages, my first guest is you have BAD DNS. In this case, I pointed my Ethernet Settings to a bad value of a DNS server.)

![mtl-ODM14][mtl-ODM14]
(Verify ODR settings and click on “Set Up” button.)

![mtl-ODM15][mtl-ODM15]
(View from the ODM Server, which now recognizes that and ODR is available.)

[mtl-ODM1]: /images/2012/07/1-mtl-ODM.png
[mtl-ODM2]: /images/2012/07/2-mtl-ODM.png
[mtl-ODM3]: /images/2012/07/3-mtl-ODM.png
[mtl-ODM4]: /images/2012/07/4-mtl-ODM.png
[mtl-ODM5]: /images/2012/07/5-mtl-ODM.png
[mtl-ODM6]: /images/2012/07/6-mtl-ODM.png
[mtl-ODM7]: /images/2012/07/7-mtl-ODM.png
[mtl-ODM8]: /images/2012/07/8-mtl-ODM.png
[mtl-ODM9]: /images/2012/07/9-mtl-ODM.png
[mtl-ODM10]: /images/2012/07/10-mtl-ODM.png
[mtl-ODM11]: /images/2012/07/12-mtl-ODR.png
[mtl-ODM12]: /images/2012/07/13-mtl-ODR.png
[mtl-ODM13]: /images/2012/07/14-mtl-ODR.png
[mtl-ODM14]: /images/2012/07/15-mtl-ODM.png
[mtl-ODM15]: /images/2012/07/11-mtl-ODM.png
