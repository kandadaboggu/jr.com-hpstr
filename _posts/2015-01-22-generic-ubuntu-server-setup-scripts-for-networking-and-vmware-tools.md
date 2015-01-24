---
layout: post
title: "Generic Ubuntu Server Setup Scripts for Networking and VMware Tools"
date: 2015-01-22T16:51:37-05:00
modified:
categories: 
description: "Scripts to get your Ubuntu Server quickly up and running"
tags:
 - Ubuntu Server
 - CLI
image:
  feature: 2015/01/22/ubuntu-logo14.png
  credit: Ubuntu
  creditlink: https://design.ubuntu.com/brand/ubuntu-logo
comments: true
share: true
---
Lately I've been doing a lot of Ubuntu Server installs for JumpStarts with their intended use to become a JSS or JDS server.  It's easy to spin up a full clone or linked clone on my laptop, but when I'm at customer's location they usually only have an ISO that was downloaded some time ago... so we're starting from scratch.  With a brand new Ubuntu Server VM, there are a few steps that you need to perform to make your life easier in the long run.

## Server Setup
On ever Ubuntu Server setup there are a couple of items that you should do first before trying to install the JDS or JSS installers.  We need the following: 

-	Server FQDN (hopefully you have already populated your internal DNS with the associated IP address)
-	Networking info (IP Address, gateway, subnet mask, etc)
-	Install openssh-server (and possibly curl) on our new VM

Assuming we have the above items, console into your new Ubuntu VM and perform a ```sudo apt-get install openssh-server``` so we can SSH into the box from Terminal (it's much easier to work with).  We can then ```scp ubuntuSetup.sh``` and execute the script to make sure you have the latest and greatest version of Ubuntu Server (since this is from an ISO that who knows how long ago) by doing a ```apt-get update``` and ```apt-get dist-upgrade```.  These updates may take some time, so be patient if you decided to run these commands.

Next we're going to input our FQDN and Networking info into the script so it can update the following files on our behalf:

-	/etc/hosts
-	/etc/hostname
-	/etc/network/interfaces

This script is available on SRT's public github repo at: [https://github.com/stonyrivertech/SRT-Public/blob/master/VMWare/ubuntuSetup.sh](https://github.com/stonyrivertech/SRT-Public/blob/master/VMWare/ubuntuSetup.sh), watch it in action below.

<iframe src="//player.vimeo.com/video/117528351?portrait=0" width="500" height="281" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe> <p><a href="http://vimeo.com/117528351">Ubuntu Server Setup Script Demo</a></p>

## VMware Tools
The next script makes sure that you have the proper VMware Tools installed on Ubuntu Server just in case you need to share a folder on your host computer to your Ubuntu server.  The storage point will be ```/mnt/hgfs/<<your folder name>>```.  This script will try to mount the CDROM on your VM, so be sure it is pointing to the VMware Linux Tools ISO by choosing Virtual Machine => Install VMware Tools.

{% highlight bash %}
#!/bin/bash

# This script was developed BY Stony River Technologies (SRT)
# ALL scripts are covered by SRT's License found at:
# https://raw.github.com/stonyrivertech/SRT-Public/master/LICENSE 

# Created by Justin Rummel
# Version 1.0.0 - 11/15/2012

# Modified by
# Version 


### Description 
# Goal is to install VMWare Tools on an Ubuntu Server.  Don't forget to mount the CD first! 

# variables
cdMNT="/mnt/cdrom/"
cdDEV="/dev/cdrom/"

### Be sure to select "Install VMWare Tools" from the "Virtual Machine" dropdown menu in VMWare Fusion
[ ! -d "${cdMNT}" ] && { echo "creating ${cdMNT}"; sudo mkdir "${cdMNT}"; } || { echo "${cdMNT} already exists.  Moving on..."; }
[ ! -d "${cdDEV}" ] && { echo "mounting ${cdDEV} to ${cdMNT}"; sudo mount "${cdDEV}" "${cdMNT}"; }

vmTGZ=`find "${cdMNT}" -name "VMwareTools*"` 2>/dev/null

cd /tmp
if [ -e "${vmTGZ}" ]; then
	cp "${cdMNT}${vmTGZ}" ./
	tar xzvf "${vmTGZ}" 
	cd vmware-tools-distrib/
	sudo apt-get install build-essential
	sudo apt-get install build-essential linux-headers-`uname -r`
	sudo ./vmware-install.pl --default
	sudo reboot
else
	echo "Something went wrong.  Stopping now."
	exit 1
fi
exit 0
{% endhighlight %}