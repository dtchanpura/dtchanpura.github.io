---
title: "Mattermost - gokrazy"
date: 2022-01-23T12:30:20+05:30
draft: false
description: This post is about how to install Mattermost Server on Raspberry Pi running gokrazy
---

Mattermost is an open source platform for secure collaboration across the entire software development lifecycle, and it can be self-hosted anywhere. ^[1]

There are guides available on setting it up on DigitalOcean/EC2 etc. which are great and those have been inspiration for this post.

To start with, we can see that the built packages are available for linux-amd64 and linux-arm64 (not metioned website), so we are off to a good start, but after checking the dependencies we can see that there are a lot of files that are needed, like web/, i18n/, etc. which is not just as simple in shipping to gokrazy.

Get things ready, we can do this by downloading the distribution from [Deployment Guide](https://mattermost.com/deploy/). The only problem is it needs arm64 variant of the distribution. With some luck, if we replace amd64 with arm64 we can get the valid link that has all archives we need.

> Note: we need to understand there are many parts in Mattermost distribution that are architecture dependent. One is the binary itself and others are the plugins that may include some Go code, e.g. Focalboard.

# Setup

Let's download the distribution locally first

```shell
[me@linux ~]$ mkdir /tmp/dist; cd /tmp/dist
[me@linux /tmp/dist]$ MATTERMOST_VERSION=6.3.1
[me@linux /tmp/dist]$ wget https://releases.mattermost.com/${MATTERMOST_VERSION}/mattermost-${MATTERMOST_VERSION}-linux-arm64.tar.gz
# use python -m http.server or dumb-http to serve it locally to be downloaded in gokrazy
[me@linux /tmp/dist]$ python -m http.server # or dumb-http
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

On gokrazy side restart breakglass service and ssh into it, also assuming there's a /perm/ with persistent storage.

```sh
/tmp/breakglass111111111 # MATTERMOST_VERSION=6.3.1
/tmp/breakglass111111111 # cd /perm/
/perm/ # wget http://IP:8000/mattermost-${MATTERMOST_VERSION}-linux-arm64.tar.gz
/perm/ # tar -xf mattermost-${MATTERMOST_VERSION}-linux-arm64.tar.gz
/perm/ # rm mattermost-${MATTERMOST_VERSION}-linux-arm64.tar.gz
```

So we have all resources that are needed, we can now update the configuration.
1. Change the path from relative to absolute.
```sh
/tmp/breakglass111111111 #  sed -i 's#"./#"/perm/mattermost#g' /perm/mattermost/config/config.json
```
2. Change the Database source as well and we should be good.

# Install

The tools that gokrazy packs can be used to install services we need.

```sh
[me@linux gokrazy]$ gokr-packer \
    -hostname mattermost.local \
    -update=yes \
    -serial_console=disabled \
    github.com/gokrazy/breakglass \
    github.com/gokrazy/serial-busybox \
    github.com/mattermost/mattermost-server/v6/cmd/mattermost
```

As Mattermost is a Go application and has a main program as well, we can just install it by adding the main package to the list of application we install.



^[1]: https://github.com/mattermost
^[2]: More details about gokrazy are available in [separate blog post](/gokrazy/2022/01/21/gokrazy/).

