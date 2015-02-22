---
layout: post
title: "Using mcxToProfile With the Casper Suite for Advanced Configuration Profiles"
date: 2015-02-20T10:48:19-05:00
modified:
categories:
description: "Don't just accept the default options for Configuration Profiles in your JSS, make your own by mcxToProfile!"
tags:
 - Casper Suite
 - CLI
 - OSX
 - Yosemite
image:
  feature: # 2048x768 for article header
  thumb: # 400x400 for Twitter
  credit:
  creditlink:
comments: true
share: true
---
JAMF Software's Casper Suite has the ability to use Configuration Profiles (Apple's preferred method for managing OSX and iOS) since version 8.0, and introduced Apple Push Notification Service ([APNS]({{ site.url }}/search?q=apns)) support since Casper Suite 8.4.  The combination of using Configurations Profiles with APNS allows administrators the ability to push management settings, which will be installed immediately and securely on their client machines.  However, Apple has been slow to provide Configuration Profiles a 1:1 feature parity that Mac Administrators were used to be able to configure through MCX via Workgroup Manager ([RIP][rip]).  There is a need to extract legacy MCX Settings and convert them into a working Configuration Profiles to manage options in a meticulous fashion: thus [mcxToProfile][m2p].

Tim Sutton ([@tvsutton][tvsutton]) created mcxToProfile to solve this problem, and this utility is not new.  In fact, the initial commit to the GitHub repo was on June 20th 2012 and the latest update was October 11th 2013!  However, I feel that too many Casper administrators believe if there is not a Configuration Profile checkbox available from the JSS then they should figure out an alternative method which doesn't include a profile.  This is not true, have options!  For example...

VPN Settings (Advanced)
---
When you configure VPN Settings (VPN Server, Authentication type, parameters, etc) via the JSS, the configuration part is easy and, more importantly, it works!  However, there are a few items that we could do that would really help the end user experience.

![VPN Settings]({{ site.url }}/images/2015/02/19/VPN-Settings.png)
{: .image-right}

-	Enable the VPN Menu Item at the top
-	Enable the "Show Time Connected" setting so people can see that their VPN session is established

These two items are not available by a single checkbox within the JSS, however, we can find the proper plist files that have these settings and use mcxToProfile to create a custom plist.

#### Find the plists
There are actually two different plist files that we need to review; one for the Menu Items and the second for the VPN Menu Item settings<sup id="fnr1-2015-02-19">[1]</sup>.  The first one for the VPN Menu item itself is found in ~/Library/Preferences/com.apple.systemuiserver.plist.  In a freshly installed system the file is relatively blank except for two lines, but if we manually create a VPN connection in System Preferences => Network and enable the "Show VPN status in menu bar" we can see:

{% highlight bash %}
Locals-Mac:~ ladmin$ defaults read ~/Library/Preferences/com.apple.systemuiserver.plist
{
    "__NSEnableTSMDocumentWindowLevel" = 1;
    "last-messagetrace-stamp" = "446068721.278329";
    menuExtras =     (
        "/System/Library/CoreServices/Menu Extras/VPN.menu",
        "/System/Library/CoreServices/Menu Extras/Displays.menu",
        "/System/Library/CoreServices/Menu Extras/Clock.menu"
    );
}
{% endhighlight %}

There is some extra stuff with the file, but we'll clean it out a little later.

Next lets take a look at ~/Preferences/com.apple.networkConnect.plist.  Again, default in a freshly installed system this file does not exist.  However, when we start toggling the VPN display options from the VPN Menu Item the file gets written with the needed key/pair attributes:

{% highlight bash %}
Locals-Mac:~ ladmin$ defaults read ~/Library/Preferences/com.apple.networkConnect.plist
{
    VPNShowStatus = 1;
    VPNShowTime = 1;
}
{% endhighlight %}

#### I Love it when a Plan Comes Together
First lets get the mcxToProfile python script downloaded onto our machine.  We can do this by opening Terminal and doing a ```git clone``` command which pulls down the script plus the README.md file with examples.

{% highlight bash %}
Locals-Mac:Desktop ladmin$ git clone https://github.com/timsutton/mcxToProfile.git
Cloning into 'mcxToProfile'...
remote: Counting objects: 129, done.
remote: Total 129 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (129/129), 32.83 KiB | 0 bytes/s, done.
Resolving deltas: 100% (63/63), done.
Checking connectivity... done.

Locals-Mac:Desktop ladmin$ cd mcxToProfile/

Locals-Mac:mcxToProfile ladmin$ ls -al
total 72
drwxr-xr-x   7 ladmin  staff    238 Feb 20 07:29 .
drwx------+  5 ladmin  staff    170 Feb 20 07:29 ..
drwxr-xr-x  13 ladmin  staff    442 Feb 20 07:29 .git
-rw-r--r--   1 ladmin  staff     31 Feb 20 07:29 .gitignore
-rw-r--r--   1 ladmin  staff   1086 Feb 20 07:29 LICENSE.md
-rw-r--r--   1 ladmin  staff   5684 Feb 20 07:29 README.md
-rwxr-xr-x   1 ladmin  staff  17320 Feb 20 07:29 mcxToProfile.py
{% endhighlight %}

With mcxToProfile we can identify multiple plist files that can be merged into one file to be imported into the JSS. This is clearly demonstrated in one of mcxToProfile examples available at [https://github.com/timsutton/mcxToProfile#example-usage](https://github.com/timsutton/mcxToProfile#example-usage).  Since my desire to display the VPN Menu Item and show the connection status are my personal suggestions to end users, I'm also applying the ``` --manage Once``` option so changes can be made by the user at a later time.  To generate our one plist file that we will import into the JSS, perform the following command:

{% highlight bash %}
Locals-Mac:~ ladmin$ ./mcxToProfile.py --plist ~/Library/Preferences/com.apple.networkConnect.plist --plist ~/Library/Preferences/com.apple.systemuiserver.plist --identifier com.local.vpnSetup --manage Once
{% endhighlight %}

This results in a nicely contained com.local.vpnSetup.mobileconfig file that has imported all of our settings from our two source plist files.  However, remember when reviewing the com.apple.systemuiserver.plist there was some extra stuff that we needed to remove; "Display.menu" and "Clock.menu" shouldn't be a part of our VPN configuration.  Open the com.local.vpnSetup.mobileconfig file in your favorite text editor and remove the lines for these two menu items.  You may also want to change the "PayloadDisplayName" value from "MCXToProfile: com.apple.networkConnect" to something more meaningful for your end users in case they review what has been installed in System Preferences => Profiles.<sup id="fnr2-2015-02-19">[2]</sup><sup id="fnr3-2015-02-19">[3]</sup>  The resulting file should look like this:

{% gist 0ac03f516bd3b81de5f6 %}

Once you Create a new Configuration Profile in the JSS by <a href="{{ site.url }}/images/2015/02/19/Upload-JSS.png">uploading our new combined plist file</a>, we can then deploy to our test environment as demonstrated in this short video:

<iframe src="//player.vimeo.com/video/120099841?portrait=0" width="500" height="281" frameborder="0"> </iframe>

Resources
---

-	[Casper Suite Series Evolution][CasperSuiteSeriesEvolution]
-	[https://github.com/timsutton/mcxToProfile][m2p]
- [https://developer.apple.com/library/ios/featuredarticles/iPhoneConfigurationProfileRef/][iPhoneConfigurationProfileRef]

## Footnotes

<div class="footnotes">
<hr />
<ol>
<li id="fn1-2015-02-19"><p>I found these plist files by doing a little Google searching.  However, Casper Admins could also use <a href="{{ site.url }}/images/2015/02/19/Composer-New-Mod.png">Composer with the "New and Modified Snapshot" method</a> to discover what files are changing when you "check a box".<a href="#fnr1-2015-02-19" class="footnoteBackLink" title="Jump back to footnote 1 in the text.">&#8617;</a></p></li>
<li id="fn2-2015-02-19"><p>The keys "__NSEnableTSMDocumentWindowLevel" and "last-messagetrace-stamp" may be able to be removed along with the other menu items.  I did not remove these keys during my testing and the VPN Menu item and it's settings worked as desired.<a href="#fnr2-2015-02-19" class="footnoteBackLink" title="Jump back to footnote 2 in the text.">&#8617;</a></p></li>
<li id="fn3-2015-02-19"><p>This step is not required for JSS Admins as you will have the chance to rename it once you import the plist into your new Configuration Policy.<a href="#fnr3-2015-02-19" class="footnoteBackLink" title="Jump back to footnote 3 in the text.">&#8617;</a></p></li>
</ol>
</div>


[rip]: http://support.apple.com/en-us/HT201651
[m2p]: https://github.com/timsutton/mcxToProfile
[tvsutton]: https://twitter.com/tvsutton
[CasperSuiteSeriesEvolution]: http://resources.jamfsoftware.com/archive/CasperSuiteSeriesEvolution.pdf
[iPhoneConfigurationProfileRef]: https://developer.apple.com/library/ios/featuredarticles/iPhoneConfigurationProfileRef/Introduction/Introduction.html
[1]: #fn1-2015-02-19
[2]: #fn2-2015-02-19
[3]: #fn3-2015-02-19
