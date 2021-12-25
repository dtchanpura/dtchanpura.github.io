---
layout: post
title: "Things I learned in 2016"
date: 2016-12-31 08:22:04
categories: technology
status: published
description: "First and Last post of 2016. Looking forward for a great new year."
display: true
tags: ["docker", "aws", "hadoop", "bigdata"]
aliases: [/technology/2016/12/31/year-insights.html]
---

## Year insights

Well a lot of things were really good about this year.

At IQM Corp. I have been given roles for infrastructure and data science. I didn't know that learning big data tools is easy. The real difficult part is the implementation and the data to work on.

## Docker<a name="docker"></a>

Docker is something which gets mixed up with virtualization but it takes a lot of time to understand it is a Container and it is so different than Virtual Machine. More information can be found at [docker.io](https://docker.io). Management can really be tough but it has been simplified by [Kubernetes](http://k8s.info/)

Lot of things gets changed when you know how it works. So earlier according to me using docker was like a virtual machine and I used it the same way. But as it turns out its a simple Container containing an application. When you get to know how to use something you suddently become the master.

I used docker for a simple REST API in [flask](https://flask.pocoo.org) once but didn't go till the production due to some hazardous side-effects it can have as the code and containers were un-tested and didn't got much time for the same.

But I have used docker extensively for [OpenVPN](https://openvpn.net). Mostly the way I have used it is from [kylemanna/docker-openvpn](https://github.com/kylemanna/docker-openvpn) repository and what a great documentation for the same. It does say following thing.
> Extensively tested on [Digital Ocean $5/mo node](http://bit.ly/1C7cKr3) and has
a corresponding [Digital Ocean Community Tutorial](http://bit.ly/1AGUZkq).

So at the end I thought its good to know about using things which can help you out and can make you more productive.

## Amazon Web Services<a name="aws"></a>

Another thing which I learned this year was AWS. Now AWS is something which can be used by an individual and also an enterprise for scaling up its infrastructure. We faced the same issues while working with normal servers we didn't got good performance with the power it had underneath. So switching to cloud was the only option left.

At that stage Google Cloud Engine was not that mature as we see it now. So starting with AWS was a good decision. More information at [Amazon Web Services](https://aws.amazon.com)

## Apache Hadoop Ecosystem (Big Data)<a name="hadoop"></a>

Well this technology is trending everywhere and we had to work on big data so we started working on it. Learning is little bit easy but when it comes to production use we had to make a lot of changes for faster computing with efficient use of resources.

Apache Hadoop is mature and has a great support both from Apache and its community. In terms of mapreduce Apache Spark is like its sequel. There are many benchmarks available and has a lots of APIs for Machine Learning, SQL, Streaming, etc. More information at [Apache Spark](https://spark.apache.org)

## Linux Kernel<a name="kernel"></a>

Linux Kernel is really huge and can take a lot time to optimize OS performance. We did some optimizations in `/proc/sys/` files of Linux operating system which can be helpful as by default its not optimized for production use.

More information can be found at [Linux Kernel Documentation for Sysctl](https://www.kernel.org/doc/Documentation/sysctl/).

## AngularJS and Web Service<a name="ws"></a>

This has been widely used now a days and I also tried the same in most of my personal projects last year and this year too. So I love working with hardware and had all three generations of Raspberry Pi. One of my project was about Music Player [Frontend](https://github.com/dtchanpura/yummy-octo-front) [Backend](https://github.com/dtchanpura/yummy-octo-musique). So here for this project's frontend is hosted on github [here](http://dcpri.me/yummy-octo-front/#/).

So backend has some endpoints like `/play`, `/next`, etc. So this AngularJS UI had events that call the same.

## WebSockets (Socket.IO) <a name="socketio"></a>

Year ended with a great new technology to learn. WebSockets API for HTML5 is really efficient way to transmit messages from Client to Server and vice-versa. It has a wrapper API which is [Socket.IO](http://socket.io)

More information at [Websocket](https://davidwalsh.name/websocket)
