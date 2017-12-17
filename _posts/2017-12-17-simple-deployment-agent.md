---
layout: post
title: Simple Deployment Agent
date: 2017-12-17T17:18:00.000Z
updated_on: 2017-12-17T18:38:55.000Z
categories: technology
status: published
description: >-
  This post describes use of the project which is a simple deployment agent
  (dep-agent). GitHub: [dtchanpura/deployment-agent](https://github.com/dtchanpura/deployment-agent)
display: true
tags:
  - devops
---

# Introduction

This post is mainly about a project which I carried out to achieve following
simple tasks.

* Listen for an update.
* Execute some predefined tasks on update.
* Secure the listener with only some clients.

To carry out these tasks the project has a listener which listens for a webhook
called from a CI server once the updated code is tested and can be deployed.
After the webhook, it executes the configured scripts or commands with
arguments on the server which can be used to download a file (archive) from
cloud and deploy to app server.

# Use

The usage is really straight forward, initialize, add configurations, modify it
(currently by editing config file) and start listener. For a common use case
following are steps.

## Get the binary

* Download the latest archive, which is relevant to you, from the releases page of GitHub repository. [https://github.com/dtchanpura/deployment-agent/releases/latest](https://github.com/dtchanpura/deployment-agent/releases/latest)

* Extract the archive
```sh
tar -xvf deployment-agent-*.tar.gz -C path/to/extract
```

* This archive contains a executable file `dep-agent` copy it to a location which
is in the PATH variable. e.g. `/usr/local/bin` or `$HOME/bin`
```sh
cp path/to/extract/dep-agent $HOME/bin
```

* Command help/usage can be invoked by running `dep-agent --help`

## Initialize and add a new configuration ➕

* If you have copied executable file to some place contained by PATH variable you can use it directly by typing dep-agent in shell, else execute it by `/absolute/path/to/extract/dep-agent`

### Initialize

* To initialize just run following command, which creates a configuration
file

```sh
dep-agent init
```

* Initialized configuration file can be found at `$HOME/.dep-agent.yaml`

### Adding a configuration

Configuration contains following things.

* Name
    * Description: Name of the project
    * Option: `--name`

* Workdir
    * Description: Working directory to execute hooks
    * Option: `--work-dir`

* Pre-hook
    * Description: Script Path to execute first
    * Option: `--pre-hook`

* Post-hook
    * Description: Script Path to execute last
    * Option: `--post-hook`

* Error-hook
    * Description: Script Path to execute in event of error
    * Option: `--error-hook`

* Whitelisted IP addresses (CIDR)
    * Description: CIDR that is whitelisted for this configuration (can be more than one)
    * Option: `--ip-cidr`

Following is a sample command to run for adding a project configuration.

```sh
dep-agent add --name name \
    --work-dir work/dir/location \
    --pre-hook /script/pre-hook.sh \
    --ip-cidr 192.168.0.0/16 # To allow
    # 192.168.0.0 to 192.168.255.255 IPs
```

This will return two things which are necessary for the accessing the listener.

```
UUID for this project is: ece419ae-8ee2-44e3-a0d3-589eae79cd27
Hash to be used for 192.168.0.0/16: Cgcf012PIoTAx9lG93N7qHg_Cg9qYM_g_TMjh690xGDS
```


* UUID which will be project's ID
* Hash which is a secret hash to authenticate for executing scripts on webhook call.

## Serving 🍽

This part is mainly to start the server which listens for webhook.

It can be configured by changing the host/port in configuration file.

Serve command comes with an option to detect change in project configurations by appending `--watch-config`.

```sh
dep-agent serve --watch-config
```

This should start a server listening.

### Systemd Service File (Recommended)

One can use following systemd file if needed to serve in background using Systemd.

```
[Unit]
Description=Deployment Agent Listener
After=network.target

[Service]
Type=simple
WorkingDirectory=/home/$USERNAME
ExecStart=/home/$USERNAME/bin/dep-agent serve --watch-config
Restart=on-abort

[Install]
WantedBy=multi-user.target
```

Replace the text $USERNAME with your username and copy it to user folder of systemd configurations. For my Debian system it works in `~/.config/systemd/user/` you can also use `/etc/systemd/user/`. Then do reload systemd configurations and start server followed by checking status.

```
systemctl --user daemon-reload
systemctl --user start dep-agent.service
systemctl --user status dep-agent.service
```

# Future work

* Add retrieving, regenerating or revoking the Hash
