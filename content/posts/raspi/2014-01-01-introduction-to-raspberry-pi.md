---
layout: post
title: "Introduction to Raspberry Pi"
categories: raspi
date: 2014-01-01 21:41:59
status: published
description: "Raspberry Pi is a small computer which is been designed in the \"UK\". I am saying it as computer but actually its a small credit card sized board which has an ARM processor which is a Broadcom SoC (system on chip) which includes a GPU, 512MB RAM, 700MHz ARMv6 CPU."
display: true
aliases: [/raspi/2014/01/01/introduction-to-raspberry-pi.html]
---
Firstly I wish you a very Happy New year. This year I was thinking of taking a new year resolution as to write at least one post every week. I know that's quite tough for me but will try my level best. I am very happy to announce that I just received a Raspberry Pi (RasPi, aliased). Raspberry Pi is a small computer which is having a powerful processor clocked at 700MHz and a 512MB of RAM. This might be too technical but believe me when you get the full introduction to RasPi you will really think to buy one.

Raspberry Pi is a small computer which is been designed in "the UK". I am saying it as computer but actually its a small credit card sized board which has an ARM processor which is a Broadcom SoC (system on chip) which includes a GPU, 512MB RAM, 700MHz ARMv6 CPU.

Now just think of those endless applications that we can make through RasPi. Though it may have lesser power than a normal PC which may have a 4GB of RAM and some Intel Processor clocked at 2GHz to 3GHz but that is also somewhat costly. But now at that cheaper cost we are getting much more that required. 

ARM. ARM is Advanced RISC Machines firstly developed by Acron Computers. They have just made the Core architechture and they license it to the manufacturers for making the end user or end prototype chip. This is the strategy of Acorn. ARM has different revisions which are known as versions for more information on ARM visit <a href="http://arm.com">ARM</a> or read about it at <a href="http://en.wikipedia.org/wiki/ARM_architecture">Wiki.</a>

One thing that comes with ARM processors is GPIOs commonly known as General Purpose Input/Output. This can be of any number depending upon the SoC's design. Now for simple applications like home-automation, etc. GPIOs come in handy. This post is for introduction and nothing more but still I will be sharing many details about the RasPi...

Following is the image of RasPi. How it looks like..

<div id="container">
<a href="/images/raspi.jpg"><img src="/images/raspi.jpg" alt="I <3 Raspberry Pi" width=90% /></a>
</div>

The next thing we need to know it what includes in the package. This following shows the different components of RasPi. Processor, 2 USB, Power Input (microusb), SDcard Slot (SDcard contains boot.img for booting up the RasPi), Audio Jack, Video out as HDMI and RCA. 

<div id="container">
<a href="http://www.raspberrypi.org/wp-content/uploads/2011/07/RaspiModelB.png"><img src="http://www.raspberrypi.org/wp-content/uploads/2011/07/RaspiModelB.png" width=90% alt="Raspberry"/></a>
</div>

The basic things you need to start a Raspberry Pi are as follows:

* Raspberry Pi, can be bought from <a href="http://element14.com">element14</a>.
* Power Supply or a simple smartphone charger with current rating greater than 700mA (for model B). 
* SDcard, here its SDHC normal digital camera memory cards. for checking if your sdcard works with Raspi visit <a href="http://elinux.org/RPi_SD_cards">elinux.org</a>(minimum of 4GB is recommended).
* Ethernet Cable or WiFi USB module (for internet connection).
* A TV or Monitor with HDMI or RCA input (for Display).
* A CASE for protection.

This was the basic things you need for setting up Raspi but now if you need to remove some of the components you can just connect to the ethernet and can use it via <a href="http://en.wikipedia.org/wiki/SSH">SSH</a> or <a href="http://en.wikipedia.org/wiki/Virtual_Network_Computing">VNC</a>.

Will continue next time with installing an image on SDCard, which can be downloaded from <a href="http://www.raspberrypi.org/downloads">RPi_Downloads</a> and updating via <code>sudo apt-get update && sudo apt-get upgrade</code>

End of Post.

