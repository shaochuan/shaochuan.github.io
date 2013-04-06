---
layout: post
title: "Every Geek Should Own One of Raspberry Pi(s)"
description: ""
category: Tutorial
tags: [Raspberry Pi, Tutorial]
---
{% include JB/setup %}

![My Raspberry Pi](/assets/images/2013-04-06-raspberry_pi.jpg "Raspberry Pi")

## Realization of Ubiquitous Computing
Several years ago, I started to hear people talking about ideas of [ubiquitous computing] (http://en.wikipedia.org/wiki/Ubiquitous_computing). One day microprocessor will become very powerful but very small, and thus we can put literally a computer inside a lot of items, like [glasses] (http://www.google.com/glass/start/what-it-does/). Mobile devices, like iPhone and iPad, help a lot to achieve this concept, but the programming platform is more constrained. For example, you don't usually have full control of your device; you cannot attach the phone to a customed device very easily. Raspbiberry Pi breaks this constraint, in that it is literally a small linux machine that essentially a lot of codes run at your desktop PC can be recompiled and run on the Raspberry Pi.

For a lot of developers, this is really exciting. Along with [General Purpose I/O] (http://en.wikipedia.org/wiki/General_Purpose_Input/Output) and USB, you can write codes (even python codes) to control voltage using [RPi.GPIO] (https://pypi.python.org/pypi/RPi.GPIO). LEDs can be controlled this way pretty easily, and if you are good at hardware, you can build your own device and write software to build a real prototype.

Matt Richardson combines the bike, Raspberry Pi and a pico projector to build a dynamic headlight which looks really awesome.

<iframe width="640" height="480" src="http://www.youtube.com/embed/Nfk1-XMASrk"
        frameborder="0" allowfullscreen="allowfullscreen">  </iframe>

I can see a lot of interesting projects like this will come out of this year with Raspberry Pi(s).

## How I Get My Raspberry Pi?
Officially, [Allied Electronics](http://www.alliedelec.com/lp/120626raso/) is selling Raspberry Pi at $35.00. However, at the time of this blog post is written, the available stock count is zero at AE. Hence, I bought mine from [amazon.com](http://www.amazon.com/Raspberry-Pi-Model-Revision-512MB/dp/B009SQQF9C/ref=sr_1_1?ie=UTF8&qid=1365235257&sr=8-1&keywords=raspberry+pi) at higher price. Despite the fact that it is more expensive at amazon.com, you have the benefits of rapid delivery and the customer service if you are unlucky to get a bad hardware.

## What You Need In Addition To Raspberry Pi

 - A compatible SD card to hold the operating system. [Here] (http://elinux.org/RPi_SD_cards) is a list of compatible SD cards. I bought a [SDSDU-032G-U46] (http://www.amazon.com/gp/product/B007BJHEWK/ref=oh_details_o02_s00_i00?ie=UTF8&psc=1) that is working pretty well.
 - A micro USB power supply. You can buy [one] (http://www.amazon.com/Motorola-Micro-USB-Home-Travel-Charger/dp/B004EYSKM8/ref=sr_1_1?s=electronics&ie=UTF8&qid=1365236584&sr=1-1&keywords=micro+usb+power+supply+raspberry+pi) online, or if you own an android phone and it is using micro USB cable too, you can use the one you already had.
 - An ethernet network cable. Otherwise, it is hard to install software package.
 - A [HDMI cable] (http://www.amazon.com/AmazonBasics-High-Speed-HDMI-Cable-Meters/dp/B003L1ZYYM/ref=sr_1_1?s=electronics&ie=UTF8&qid=1365236748&sr=1-1&keywords=hdmi+cable) or composite video cable and a monitor/display with HDMI port. This could be optional because once you have set up your Raspberry Pi, you can remote login it.
 - A USB keyboard and mouse. Just as HDMI cable, after the Raspberry Pi booted, you can always use remote login to control your Raspberry Pi.

## Prepare Your SD Card

I downloaded [Raspbian wheezy] (http://www.raspberrypi.org/downloads) and followed the [installation instructions] (http://elinux.org/RPi_Easy_SD_Card_Setup) without encountering too many problems.

Here are what I did on a Macbook Pro (Mac OS 10.7.2).

Use [diskutil] (http://developer.apple.com/library/mac/#documentation/Darwin/Reference/ManPages/man8/diskutil.8) to list your disks.

		$ diskutil
		/dev/disk0
		   #:                       TYPE NAME                    SIZE       IDENTIFIER
		   0:      GUID_partition_scheme                        *500.1 GB   disk0
		   1:                        EFI                         209.7 MB   disk0s1
		   2:                  Apple_HFS Macintosh HD            499.2 GB   disk0s2
		   3:                 Apple_Boot Recovery HD             650.0 MB   disk0s3
		/dev/disk1
		   #:                       TYPE NAME                    SIZE       IDENTIFIER
		   0:     FDisk_partition_scheme                        *31.9 GB    disk1
		   1:             Windows_FAT_32                         58.7 MB    disk1s1
		   2:                      Linux                         31.9 GB    disk1s2

In my case the disk path of SD card is /dev/disk1. Now unmount it using the following command,

		$ diskutil unmountDisk /dev/disk1
		Unmount of all volumes on disk1 was successful

Now, using [dd] (https://developer.apple.com/library/mac/#documentation/Darwin/Reference/Manpages/man1/dd.1.html) command to trasnfer the downloaded file to the SD card,

		$ sudo dd bs=1m if=2013-02-09-wheezy-raspbian.img of=/dev/disk1
		(now you can go facebooking for a while...)

After it finishes the transfer, insert your SD card into your Raspberry Pi and all other peripherals first and then plug in the micro USB with power. Note that once you plug in the power, it will start to boot.

Viola! Say hello to Raspberry Pi!

![Raspberry Pi Desktop](/assets/images/2013-04-06-raspberry_desktop.jpg "Raspberry Pi Desktop")

Enjoy your hacking with Raspberry Pi! :)
