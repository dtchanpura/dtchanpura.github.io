---
layout: post
title: DNS and Systemd - Resolved!
date: 2020-08-13T09:29:10.000Z
lastmod: 2020-08-13T09:49:51Z
categories: technology
status: beta
description: >-
  Pun intended.
display: true
tags:
  - devops
aliases: [/technology/2020/08/13/dns-problems.html]
---

# DNS and Systemd

Have you ever faced an issue that you get a good speed in https://speedtest.net or https://fast.com but still while opening any new page it takes a while?

If the answer to that question is yes, then this post is worth reading and implementing.

# Background

DNS has always been quite complicated for me, and somehow I always complain about ISP or the website that it's not working.

I am not sure if everyone will be having slow router/modem which doesn't do DNS resolution properly, but that was in my case.

So the problem was something like this

```
~ ❯ time nslookup google.com
Server:         127.0.0.53
Address:        127.0.0.53#53

Non-authoritative answer:
Name:   google.com
Address: 142.250.67.142
;; connection timed out; no servers could be reached


nslookup google.com  0.02s user 0.01s system 0% cpu 15.809 total
~ took 15s ❯ time nslookup dcpri.me
Server:         127.0.0.53
Address:        127.0.0.53#53

Non-authoritative answer:
Name:   dcpri.me
Address: 185.199.111.153
Name:   dcpri.me
Address: 185.199.108.153
Name:   dcpri.me
Address: 185.199.109.153
Name:   dcpri.me
Address: 185.199.110.153
;; connection timed out; no servers could be reached


nslookup dcpri.me  0.01s user 0.02s system 0% cpu 20.033 total
```

Resolving DNS taking more than a second is bad for the experience. In my case
from terminal it took around 10-15s.

On browser I was able to see the same behaviour. It was little less, something
in range of 5-10s.

# Problem

The main problem as seen from the error message it says connection timed out,
what could be the actual reason for timing out.

So to debug a bit, I checked the same command in one of the VPS server which I
was running at the moment.

```
[root@bash-running-server ~]# time nslookup google.com
Server:         x.x.x.x
Address:        x.x.x.x#53

Non-authoritative answer:
Name:   google.com
Address: 172.217.13.174
Name:   google.com
Address: 2607:f8b0:4020:806::200e


real    0m0.342s
user    0m0.060s
sys     0m0.040s
```

From the command output I immediately understood that DNS resolution was timing
out for resolving the IPv6 part of the DNS Record.

# Solution

If you notice in the first section, we can see the `Server` entry was
127.0.0.53 which means there's a resolver in local system which helps resolve
DNS faster (yeah, right! :rolling_eyes:).

This was mostly because of resolver which wasn't able to get the things right
and quick and wait a lot for IPv6 addresses.

After digging deep in it, I was able to see that it uses `systemd-resolved`
stub resolver, more details were found in `/etc/resolv.conf`.

I saw the man page and ran some random commands and it all became really quick.

```
# Using CloudFlare's DNS on my WiFi Interface wlp5s0
root@mypc:~# systemd-resolve --set-dns=1.1.1.1 --interface=wlp5s0
```

Now it just adds 1.1.1.1 as the default DNS to look for. I tried running couple
of more commands to see what was the default value 

That's it. After doing this change nslookup commands loaded up quickly as
expected and also resolving the IPv6.

```
~ ❯ time nslookup google.com
Server:		127.0.0.53
Address:	127.0.0.53#53

Non-authoritative answer:
Name:	google.com
Address: 172.217.26.238
Name:	google.com
Address: 2404:6800:4009:805::200e

nslookup google.com  0.02s user 0.01s system 33% cpu 0.078 total
~ ❯ time nslookup dcpri.me
Server:		127.0.0.53
Address:	127.0.0.53#53

Non-authoritative answer:
Name:	dcpri.me
Address: 185.199.110.153
Name:	dcpri.me
Address: 185.199.109.153
Name:	dcpri.me
Address: 185.199.111.153
Name:	dcpri.me
Address: 185.199.108.153

nslookup dcpri.me  0.01s user 0.01s system 53% cpu 0.045 total
```
