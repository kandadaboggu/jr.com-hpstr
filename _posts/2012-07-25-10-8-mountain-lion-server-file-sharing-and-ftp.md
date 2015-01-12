---
title: '10.8 Mountain Lion Server: File Sharing and FTP'
author: Justin Rummel
layout: post
permalink: /10-8-mountain-lion-server-file-sharing-and-ftp/
shorturl:
  - http://j.mp/NJtOuq
tags: 
  - Apple
  - Mt Lion
  - OSXS
comments: true
image:
  feature:
  credit:
  creditlink:
---
File Sharing
------------
In terms of File Sharing, there is really nothing different from Lion to Mt Lion. There is a bonus for iOS users in EDU in that you can now create a WebDAV "DropBox" for students submitting their assignments. Follow [this kbase for setting up DropBox shares][PH8034] and you will soon be able to have students store their assignments on your server.

[PH8034]: http://support.apple.com/kb/PH8034

![1-mtl-dropbox][1-mtl-dropbox]

![2-mtl-dropbox][2-mtl-dropbox]

![3-mtl-dropbox][3-mtl-dropbox]

FTP
---
FTP has returned! Actually, returning doesn’t really deserve an exclamation point IMHO, however, there were several complains in the Mac Community that this item was missing during the transition to Lion.  The FTP interface is not full feature (but neither was Snow Leopard’s) as you can only identify one folder as being a FTP share. You can select any folder that has already been configured in File Sharing as an AFP/SMBX share, the Websites folder located at /Library/Server/Web/Data/Sites/, or choose any location by selecting the "Custom" option.

![4-mtl-dropbox][4-mtl-dropbox]

Footnotes
---------
*   For what ever reason Apple took FTP out of Server Admin and did not provide users with a replacement. There are may ways to re-enable in Lion, but now as of Mt Lion we have a full port of the Server Admin features (which wasn’t really much to begin with).

[1-mtl-dropbox]: /images/2012/07/1-mtl-dropbox.png
[2-mtl-dropbox]: /images/2012/07/2-mtl-dropbox.png
[3-mtl-dropbox]: /images/2012/07/3-mtl-dropbox.png
[4-mtl-dropbox]: /images/2012/07/1-mtl-FTP.png
