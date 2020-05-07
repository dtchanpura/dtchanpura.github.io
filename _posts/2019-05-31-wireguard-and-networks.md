---
layout: post
title: WireGuard® - Simple yet powerful VPN
date: 2019-05-31T00:00:00.000Z
updated_on: 2019-05-31T00:00:00.000Z
categories: technology
status: published
description: >-
  This post describes how to use WireGuard® VPN and how it can be used to connect
  to different networks.
display: true
tags:
  - devops
---

This post will mainly include a small introduction about WireGuard® and most of the part will be about how I use it currently.

# What is WireGuard^®?

WireGuard® is an alternative to OpenVPN for me. But from their website it has a lot of other features.
> [WireGuard^®](https://www.wireguard.com) is an extremely simple yet fast and modern VPN that utilizes state-of-the-art cryptography. It aims to be faster, simpler, leaner, and more useful than IPsec, while avoiding the massive headache. It intends to be considerably more performant than OpenVPN. WireGuard is designed as a general purpose VPN for running on embedded interfaces and super computers alike, fit for many different circumstances. Initially released for the Linux kernel, it is now cross-platform (Windows, macOS, BSD, iOS, Android) and widely deployable. It is currently under heavy development, but already it might be regarded as the most secure, easiest to use, and simplest VPN solution in the industry.

# How I know about it?

I have been following WireGuard® from some time, even joined their mailing list to get some updates on the cross platform tools which they are developing. It was weird how I got to know about this product, I have been using Pass and Cgit in my personal projects and it was helping me a lot. I was curious about other projects the same developer did, that's when I was able to see WireGuard® in his [repository](https://git.zx2c4.com/). I was like, yes I need some alternative to OpenVPN which was bulky and little hard to maintain for a beginner. I tried it few times and didn't use it well as it was not entirely cross platform, as I use macOS at work, servers are Linux and have a phone from Apple so it makes it really difficult to use it everywhere.

Recently I checked and WireGuard® officially launched all the apps. It includes macOS, iOS, Android, etc. that made me try it again.

After a lot of failures I was able to successfully connect my iPhone to a small Scaleway server which I used for OpenVPN earlier and now I have just WireGuard® in it.

# How I use it?

As I said I use in through one small Scaleway server which is Linux and it has the wireguard kernel module installed.

```
$ wg
interface: wg0
  public key: anxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxHU=
  private key: (hidden)
  listening port: 65530

peer: EKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxmw=
  endpoint: 106.x.x.x:65530
  allowed ips: 192.168.2.10/32
  latest handshake: 14 hours, 6 minutes, 32 seconds ago
  transfer: 253.50 MiB received, 3.80 GiB sent

peer: Ndxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx1E=
  endpoint: 122.x.x.x:65530
  allowed ips: 192.168.2.11/32
  latest handshake: 5 days, 10 hours, 25 seconds ago
  transfer: 19.84 MiB received, 102.31 MiB sent
```

This above snippet is an output from
