---
title: "Restic Rest Server - gokrazy"
date: 2022-08-31T23:28:00+05:30
draft: false
description: This post is a guide on using Restic's rest-server gokrazy appliance.
---

This blog post is mainly a guide on using Restic's rest-server with gokrazy
appliance, it also supports mounting a disk if that is not in the /perm
directory.

# Introduction

Restic is a modern backup program for most of the Operating systems and more
information about it is available on [restic.net](https://restic.net).

Restic provides backends or repository options where the backups can be stored,
one of that is [Rest Server](https://github.com/restic/rest-server).

# Disk Setup

Before running the `rest-server` we need to setup the disk so that it can be used directly.

## Directory

We need a directory inside the external disk which can be used. For example we can use a
directory named `restic/` which will have all the named directories by username and other
configurations.

```bash
cd /run/media/username/diskname # Path where the new disk is mounted.
mkdir restic/                   # Create a path which can be used as storage
```

## Authentication

If rest-server is used in authentication mode the directory should have `.htpasswd` file with
bcrypt hashed password. This can be carried out by running following command.

```bash
touch restic/.htpasswd                  # Create a blank file .htpasswd in restic/ directory
htpasswd -B restic/.htpasswd username   # Replace username with the username one wants to use
```

For all the users needed for authentication we can repeat the last command.

# gokrazy Setup

gokrazy does not support mounting a disk directly, so [the project](https://github.com/that-awesome-organization/gokrazy-restic-rest-server)
we are going to use will wrap the original arm64 binary of restic's rest-server
which is put in `/usr/local/bin` with mount options.

Before we begin, we setup the directory where we can keep package files for
gokrazy appliance. Here the name I have given is *rpi4-gorilla*.

```bash
go mod init rpi4-gorilla                # this is an arbitrary name

# go get the latest release of the gokrazy's rest server distribution
go get development.thatwebsite.xyz/gokrazy/restic-rest-server@latest
```

## Mount Specifications

This program uses three environment variables for this specification

* MNT_SOURCE: source device for mounting.
* MNT_TARGET: target directory where the device should be mounted.
* MNT_FSTYPE: filesystem type of the device, e.g. exfat, ext4, vfat, etc...

To use these variables there needs to be the following file in the directory where the gokr-packer
command is being run.

```bash
# in the same directory as go.mod

mkdir -p env/development.thatwebsite.xyz/gokrazy/restic-rest-server/
cat << EOF > env/development.thatwebsite.xyz/gokrazy/restic-rest-server/env.txt
MNT_SOURCE=/dev/sda1
MNT_TARGET=/perm/rest-server
MNT_FSTYPE=exfat
EOF
```

## Flags

This program transparently passes all arguments to rest-server command and also the stdout/stderr
from it, so the flags or arguments which we want to pass will be actually going in to rest-server

To add flags we can just create flags.txt same as the env.txt

```bash
mkdir -p flags/development.thatwebsite.xyz/gokrazy/restic-rest-server/
cat < EOF > flags/development.thatwebsite.xyz/gokrazy/restic-rest-server/flags.txt
--path
/perm/rest-server/restic
```

Here we are defining where the root of the rest-server should be available. Also in case if there's
need to add more arguments/flags just add it in this file separated by new lines.


### Enabling Private Access to repositories

Other flags can be `--private-repos` which enables authentication and prevents anyone accessing the
repository without basic auth.

### Enabling Prometheus Metrics

Passing `--prometheus` enables the Prometheus `/metrics` endpoint, but if that is used with `--private-repos`
a new user should be created with name metrics to access the endpoint.

## Other options

In some cases the command/program will run before the /perm/ directory is mounted and that will
make it fail and retry after a second. To avoid that, we can have a delay in startup using
waitforclock feature.

```bash
mkdir -p waitforclock/development.thatwebsite.xyz/gokrazy/restic-rest-server/
touch waitforclock/development.thatwebsite.xyz/gokrazy/restic-rest-server/waitforclock.txt
```

These commands will actually instruct gokrazy to start the rest-server only after the NTP has
synced the clock, which from my observations is also after mounting the /perm/ directory.

# Bonus

The `MNT_SOURCE` environment variable also accepts `UUID` strings of disks, it is something that I
use in my backup storage, but it has not been tested properly with other devices.

## The Problem

This is more like a problem specific to my use-case, but there's no harm in adding some details about it here.

I had two external hard drives lying around of 500 GB storage each, using them individually will create multiple
directories and may not help taking my laptop's full backup as a whole.

So I decided to find a way to use those two as a single storage with btrfs.

*The problem doesn't end there...*

### `btrfs` Support

The gokrazy's default [kernel distribution](https://github.com/gokrazy/kernel) doesn't have `btrfs` support, and as this usecase was very specific, I didn't add the feature there, instead created my own [kernel distribution](https://github.com/that-awesome-organization/gokrazy-kernel) with `btrfs` support and other features that may help in future.

References:
* [Using Btrfs with Multiple Devices - btrfs Wiki](https://btrfs.wiki.kernel.org/index.php/Using_Btrfs_with_Multiple_Devices)

### Devices and Mount Data

As gokrazy is all `go` code, I had issues mounting the `btrfs` devices using the `syscall.Mount`. It needs a data string that has something like `device=/dev/sda,device=/dev/sdb`.

That sounds easy, to mitigate that I added an extra environment variable `MNT_DATA` that passes the given string to `syscall.Mount` call and it worked, but failed intermittently.

The issue was that the device path changed after reboot or crash, like `/dev/sda` would become `/dev/sdc` and the order was random as I also have been booting it off of an USB Drive.

### UUID Support

The above issue led me to use UUIDs for fetching devices with actual UUID and enable that from code. This was actually simple in testing and assumptions but not that much when I wrote the whole thing in code.

I couldn't find the best way to find UUID of a device or list devices attached and their UUIDs, so I tried to go in util-linux space and tried blkid.

As gokrazy has busybox it doesn't have blkid binary (or I didn't find a way to access it from Go code), which means I needed to add that binary in the PATH that can be then used in Go code via `exec.Command`, then parse the output to get UUID vs Device Path.

And then it worked.

Note: The binary that is included in the distribution is static build of blkid from [util-linux](https://git.kernel.org/pub/scm/utils/util-linux/util-linux.git) with config options as `--disable-all-programs --enable-blkid --enable-libblkid --enable-static-programs=blkid`.

I will add that to GitHub as and when time permits.

This is the Dockerfile:

```Dockerfile
FROM debian:latest

WORKDIR /build

RUN apt-get update -y && apt-get install -y wget build-essential bison automake pkg-config libtool gettext autopoint

COPY build.sh .

ENTRYPOINT ["/build/build.sh"]
```

and here is the build.sh

```bash
VERSION=$1
wget --progress=dot:mega https://git.kernel.org/pub/scm/utils/util-linux/util-linux.git/snapshot/util-linux-${VERSION}.tar.gz

tar -xf util-linux-${VERSION}.tar.gz

cd /build/util-linux-${VERSION}

./autogen.sh && ./configure --disable-all-programs --enable-blkid --enable-libblkid --enable-static-programs=blkid

make

mkdir -p /shared
cp blkid.static /shared
```

Build command was 

```bash
podman build -t util-linux:base -f Dockerfile   # Build Image
podman run -ti --rm util-linux:base 2.38.1      # Run the build for util-linux
```

