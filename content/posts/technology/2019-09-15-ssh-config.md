---
layout: post
title: SSH Config
date: 2019-09-15T11:18:00.000Z
lastmod: 2019-09-15T11:18:00.000Z
categories: technology
status: published
description: >-
  This post describes how to use SSH Config.
display: true
tags:
  - devops
aliases: [/technology/2019/09/15/ssh-config.html]
---

# SSH Config

I have been across this about two years back and I thought everyone would know how to use it. But turns out, at work I find it very useful and many of my co-workers are not aware of it entirely.

This post will talk a bit about `ssh_config` and how it can make your life easier, assuming you use SSH very frequently. Also being a \*nix user I don't know if this will be useful for Microsoft Windows users.

I use one laptop for work related stuff which comprises mostly connecting to, debugging and setting up AWS EC2 Instances. Now when it comes to connecting to any instance I used to type a long command as follows.

```bash
ssh -i ~/that-access-key.pem ec2-user@x.x.x.x
```

This should look simple like we can add a bash alias or something to it. But the bigger problem comes when you have a custom AMI and it uses different port sometimes.

```bash
ssh -i ~/that-access-key.pem ec2-user@x.x.x.x -p 1234
```

Once again if there's different user, ssh private key or port for different accounts like.

```bash
ssh -i ~/that-key-1.pem ubuntu@10.0.x.x -p 1234
ssh -i ~/that-key-2.pem admin@10.1.x.x -p 1231
```

This just makes life bit messy to just SSH into a server. Think about if there's something on fire or a production bug.

\*drum roll\* :drum:

Let's handle this with `ssh_config`. To get more details about `ssh_config` you can do `man ssh_config` but most handy and useful part should be covered in this post.

## Grouping hosts

The configuration can be done at a all host level (\*) which would be the default configurations for all the hosts you SSH to. There is a possiblity to group hosts together and then apply configurations to that group of hosts.

So following are some examples of groups

```
# All hosts
Host *
...

# Single host
Host github.com
...

# Group of hosts
Host 10.0.*.*
...

# Another group
Host 10.1.*.*
...
```

## Actual configurations - the fun part

These are basically all the configurations which are supported by SSH Client. The list is really big but what I have used till now are as follows


* **Port** Specifies the port number to connect on the remote host.  The default is 22.

* **User** Specifies the user to log in as.  This can be useful when a different user name is used on different machines.  This saves the trouble of having to remember to give the user name on the command line.

* **IdentityFile** It is essentially Private Key file location. Following is the official documentation from `ssh_config` manual page
> Specifies a file from which the user's DSA, ECDSA, Ed25519 or RSA authentication identity is read.  The default is `~/.ssh/id_dsa`, `~/.ssh/id_ecdsa`, `~/.ssh/id_ed25519` and `~/.ssh/id_rsa`.  Additionally, any identities represented by the authentication agent will be used for authentication unless IdentitiesOnly is set.  If no certificates have been explicitly specified by CertificateFile, ssh(1) will try to load certificate information from the filename obtained by appending -cert.pub to the path of a specified IdentityFile.

* **StrictHostKeyChecking** Following is the official documentation from `ssh_config` manual page
> If this flag is set to yes, ssh(1) will never automatically add host keys to the `~/.ssh/known_hosts` file, and refuses to connect to hosts whose host key has changed.  This provides maximum protection against man-in-the-middle (MITM) attacks, though it can be annoying when the `/etc/ssh/ssh_known_hosts` file is poorly maintained or when connections to new hosts are frequently made.  This option forces the user to manually add all new hosts.
If this flag is set to `accept-new` then ssh will automatically add new host keys to the user known hosts files, but will not permit connections to hosts with changed host keys.  If this flag is set to `no` or `off`, ssh will automatically add new host keys to the user known hosts files and allow connections to hosts with changed hostkeys to proceed, subject to some restrictions.  If this flag is set to ask (the default), new host keys will be added to the user known host files only after the user has confirmed that is what they really want to do, and ssh will refuse to connect to hosts whose host key has changed.  The host keys of known hosts will be verified automatically in all cases.

## Finally

So final ssh config should look like this. By default `ssh` client looks in `/etc/ssh/ssh_config` and overriding those configurations from `~/.ssh/config`.

```
Host 10.0.*.*
  Port 1234
  User ubuntu
  IdentityFile ~/that-key-1.pem
Host 10.1.*.*
  Port 1231
  User admin
  IdentityFile ~/that-key-2.pem
```

## Bonus Tip

I have some servers which have different access keys and use `git push` to that server. So essentially I have to override the port and ssh private key whenever I am pushing it to that place. I have added that Host in configuration and now it pushes flawlessly.
