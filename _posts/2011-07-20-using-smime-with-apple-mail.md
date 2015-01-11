---
title: Using S/MIME With Apple Mail
author: Justin Rummel
layout: post
permalink: /using-smime-with-apple-mail/
shorturl:
  - http://j.mp/qnJpvQ
categories:
  - Apple
  - certificates
  - Lion
  - OSX
  - OSXS
  - smime
comments: true
---
Most likely you have already figured out how to use your new email certificate in Mail as my article on [How to get a free S/MIME Certificate][freeSMIME] was posted almost a month ago.

[freeSMIME]: /acquiring-a-smime-certificate-for-free/

There are no settings within Apple’s Mail Preference to select your Certificate as your certificate is based off of your email address. All you need to do is simply start a new mail message and ensure you select the correct Signed and/or Encrypt flags.

![signed][signed]

[signed]: /images/2011/07/signed-and-encrypted-email.png

![uncheck][uncheck]

[uncheck]: /images/2011/07/uncheck-disabled-email.png '"X" is BAD!'

You should now see two icons on the right hand side of your messages. On a new message the Encryption Icon (Lock) will be greyed out. You need the recipient’s S/MIME public certificate before you can send an encrypted email message (this goes back to [my first post][what] referencing the exchange between the CPA and sending tax documents). The most important thing is to ensure the “Check Mark” icon is visible vs. a white “X” (meaning that your message is not being signed). This starts propagating your new email signature to your email contacts so if they choose to send you an encrypted message, it will be possible.

[what]: /what-is-smime-email-and-why-should-i-be-using-it/

A sample email exchange could be shown as below (Sorry for the multiple “Justin Rummel” address… one is for work and one is my personal account):

![emailEx][emailEx]

[emailEx]: /images/2011/07/email-examples.png