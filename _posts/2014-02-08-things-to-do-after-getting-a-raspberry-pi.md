---
layout: post
title: "Things to do after getting a Raspberry Pi."
categories: raspi
date: 2014-02-08 00:41:59
status: published
description: "Raspberry Pi is a small computer, but with lesser processing power so we cannot boot a Windows or Other Biggies. So which OSes are supported? We'll look into it in the following post."
display: true
---
So as the title says "What to do after getting a RasPi" the post will lead you to many ways of starting a RasPi <a title="i mean booting it up">(*)</a>.

Raspberry Pi is a small computer, but with lesser processing power so we cannot boot a Windows or Other Biggies (:-P directly). So which OSes are supported?

There are most OSes which are working on a normal PC can be ported to Raspi, but with some predefined constraints in terms of speed and performance. But still the question remains unanswered .. Which OSes does Raspi support? The following Linux Distros which have been officially ported to Raspi's hardware profile namely.

* <img src="/images/Raspbian_web.png" height=25 />Debian (Raspbian)
* <img src="/images/Pidora_web.png" height=25 />Fedora (Pidora)
* <img src="/images/Raspbmc_web.png" height=25 /><img src="/images/OpenELEC_web.png" height=25 />XBMC (RaspBMC, OpenELEC)
* <img src="/images/arch_web.png" height=25 />Arch Linux
* <img src="/images/RISC_OS_web.png" height=25 />RISC OS (Faster and Compact)
* NOOBS (New Out Of the Box Software)

So first task would be to select the OS. Now its depending on the function or application. You can define your application as if what is your purpose of the using Raspi. Now all have their pros and cons as discussed below. But before that let me brief you with the steps.

1. Selecting and Getting the Prerequisites like the hardware components as said earlier.
2. SD card with Burned Image on it of desired distribution.
3. That's It, Boot it Up.

Following are the steps in detail..

### Selecting and Getting the Prerequisites:

I have also said in previous post and also repeating the list. It requires following things

* Raspberry Pi, can be bought from element14.
* Power Supply or a simple smartphone charger with current rating greater than 700mA (for model B).
* SDcard, here its SDHC normal digital camera memory cards. for checking if your sdcard works with Raspi visit elinux.org(minimum of 4GB is recommended).
* Ethernet Cable or WiFi USB module (for internet connection).
* A TV or Monitor with HDMI or RCA input (for Display).
* A CASE for protection.

After getting above things you need to connect all of them and configure it.

This is the main task of whole setup process, well it does come only after buying a Raspi <a title=":-P and sd card">*</a>.  

But the question is which OS to select. We can find it easy of we cross the SD card images available for download from raspberrypi.org/downloads. So defining the goal is important.
If the purpose is to convert TV into a smart TV or want to watch some movies and listen to music we can just use RaspBMC or OpenELEC. Which makes Raspi into a Media Center.

The next is the normal computer Which is a Linux computer functioning normally and can program or run some applications that can be ran on any computer OS such as Debian or Arch Linux or Fedora. so the choice is yours if you are new you recommended is Raspbian the Debian variant with LXDE installed on it.

### SD CARD setup:

So as selection is done we need to format it and flash the image downloaded from Raspberry Pi's website. The steps will be ...

* Format the sd card.
* Download and flash the .img file on the formatted sd card. This can be done by many ways so for ..

  * Windows users they should download the SD Formatter for formatting the card or a normal formatting tool can also work and  <a href="http://sourceforge.net/projects/win32diskimager"> Win32DiskImager</a> for flashing the image.
  * Linux users can directly use command line <code>dd if=/path/to/image.img of=/dev/sdX </code>. Beware of this sdX X is the name of SD card. So to know which one is the SD card you can run commands such as <code>lsblk</code> or <code>fdisk -l</code> then depending upon the output select the SD card and run the command <code>dd</code>. There's also one application named Image Writer that can also help. But before flashing do format the SD card.

So we now have the SD card with image flashed in it. All we need is to boot it up.

1. Plug the SD card in to the slot
1. Connect the charger or power supply into that mini USB connector.
1. Ethernet Cable if necessary
1. Connect RCA or HDMI cable to TV for display.
1. Insert the USB keyboard and mouse in to the slot.

If everything goes well after starting up you will be asked for username and password. they are given in description of downloads. Here I have used Raspbian so its <code>pi/raspberry</code>.

Then it should show <code>pi@raspberrypi ~ $ </code> if you're connected to internet you can just try to type in these commands for making up to date softwares and applications.

<code>sudo apt-get update && sudo apt-get upgrade</code> for Raspbian

<code>pacman -Syu</code> for Arch

End of Post
