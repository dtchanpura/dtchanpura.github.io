---
title: "Drone Runner Docker - gokrazy"
date: 2022-04-29T23:19:30+05:30
draft: false
description: This post is about how to run Drone Runner Docker on Raspberry Pi running gokrazy.
---

This post is not a complete guide to run drone-runner-docker with all code setup, instead if shows what problems I faced and how I fixed them.

# Purpose

Drone is a CI Tool and to build or run the tasks mentioned in .drone.yml we need runners. There are different types of runners, docker, kubernetes, exec (script), etc.

These workers are needed to be running on any instances where the main server is accessible.

In an ideal scenario, following command should start up the runner.

```
# docker run --detach \
  --volume=/var/run/docker.sock:/var/run/docker.sock \
  --env=DRONE_RPC_PROTO=https \
  --env=DRONE_RPC_HOST=drone.company.com \
  --env=DRONE_RPC_SECRET=super-duper-secret \
  --env=DRONE_RUNNER_CAPACITY=2 \
  --env=DRONE_RUNNER_NAME=my-first-runner \
  --publish=3000:3000 \
  --restart=always \
  --name=runner \
  drone/drone-runner-docker:1
```

## Problem 1: Daemon needs a sock.

Docker runs as a daemon, and we need the /var/run/docker.sock for it to run. The reason being, internally the drone-runner-docker binary will run docker containers based on the configuration from inside the main container.

For podman that's not the case, to fix it we need to run the podman as service, as follows.

```
# mkdir /perm/podman
# podman system service --time 0 unix://perm/podman/podman.sock
```

The above command will run the podman as service or daemon on /perm/podman/podman.sock.

So now it should work right?

```
# podman run --detach \
  --volume=/perm/podman/podman.sock:/var/run/docker.sock \
  --env=DRONE_RPC_PROTO=https \
  --env=DRONE_RPC_HOST=drone.company.com \
  --env=DRONE_RPC_SECRET=super-duper-secret \
  --env=DRONE_RUNNER_CAPACITY=2 \
  --env=DRONE_RUNNER_NAME=my-first-runner \
  --publish=3000:3000 \
  --restart=always \
  --name=runner \
  drone/drone-runner-docker:1
time="2022-04-29T17:52:16Z" level=info msg="starting the server" addr=":3000"
time="2022-04-29T17:52:17Z" level=info msg="successfully pinged the remote server"
time="2022-04-29T17:52:17Z" level=info msg="polling the remote server" arch=arm64 capacity=2 endpoint="https://drone.company.com" kind=pipeline os=linux type=docker
```

Wow! the runner works!! But it's not that simple as it sounds. After scheduling some jobs here, it gave different error. Let's go to Problem 2.

## Problem 2: /etc/cni/net.d whaaaa?

After running one task, it gave following error.

```
Error response from daemon: error opening "/etc/cni/net.d/cni.lock": read-only file system
```

Now I expected that, gokrazy has read-only root system, so /etc/ is not writable. But why does this happen?

The answer is, the binary drone-runner-docker will start a new container with a new network, and it isn't able to create the file it needs to create a new network. (that's what I am assuming.)

To solve it we can override that using `--cni-config-dir`, so we can copy the file(s) from /etc/cni/net.d/ to /perm/podman/net.d/.

```
# mkdir /perm/podman/net.d/
# cp -aR /etc/cni/net.d/* /perm/podman/net.d/
# rm /perm/podman/podman.sock
# podman --cni-config-dir /perm/podman/net.d system service --time 0 unix://perm/podman/podman.sock
```

Running Container

```
# podman run --detach \
  --cni-config-dir /perm/podman/net.d \
  --volume=/perm/podman/podman.sock:/var/run/docker.sock \
  --env=DRONE_RPC_PROTO=https \
  --env=DRONE_RPC_HOST=drone.company.com \
  --env=DRONE_RPC_SECRET=super-duper-secret \
  --env=DRONE_RUNNER_CAPACITY=2 \
  --env=DRONE_RUNNER_NAME=my-first-runner \
  --publish=3000:3000 \
  --restart=always \
  --name=runner \
  drone/drone-runner-docker:1
time="2022-04-29T18:13:39Z" level=info msg="starting the server" addr=":3000"
time="2022-04-29T18:13:39Z" level=info msg="successfully pinged the remote server"
time="2022-04-29T18:13:39Z" level=info msg="polling the remote server" arch=arm64 capacity=2 endpoint="https://drone.company.com" kind=pipeline os=linux type=docker
```

Works. that is fixed now.

## Problem 3: "ip6tables" or "iptables" :laughing:

After that I encountered the error as follows.

```
Error response from daemon: error configuring network namespace for container blahblahblah: error adding pod drone-blahblah_drone-blahblah to CNI network "drone-blahblah": could not initialize iptables protocol 1: exec: "ip6tables": executable file not found in $PATH
```

Now it's actually the same binary, we just don't have ip6tables link to iptables, which is a simple problem. One solution is to add /perm/podman/bin to path and add soft link of iptables to ip6tables

```
# mkdir -p /perm/podman/bin
# ln -sf /user/iptables /perm/podman/bin/ip6tables
# export PATH=$PATH:/perm/podman/bin
# podman --cni-config-dir /perm/podman/net.d system service --time 0 unix://perm/podman/podman.sock

### in another terminal

# export PATH=$PATH:/perm/podman/bin
# podman run --detach \
  --cni-config-dir /perm/podman/net.d \
  --volume=/perm/podman/podman.sock:/var/run/docker.sock \
  --env=DRONE_RPC_PROTO=https \
  --env=DRONE_RPC_HOST=drone.company.com \
  --env=DRONE_RPC_SECRET=super-duper-secret \
  --env=DRONE_RUNNER_CAPACITY=2 \
  --env=DRONE_RUNNER_NAME=my-first-runner \
  --publish=3000:3000 \
  --restart=always \
  --name=runner \
  drone/drone-runner-docker:1
time="2022-04-29T18:21:13Z" level=info msg="starting the server" addr=":3000"
time="2022-04-29T18:21:13Z" level=info msg="successfully pinged the remote server"
time="2022-04-29T18:21:13Z" level=info msg="polling the remote server" arch=arm64 capacity=2 endpoint="https://drone.company.com" kind=pipeline os=linux type=docker
```

Aaaand I was able to see one of the build passing! That's enough for me :happy:

![drone-ci screenshot](/static/images/gokrazy-drone-runner-docker.png)