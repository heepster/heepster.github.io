---
layout: post
title: "How does Ping work?"
category: networking
---

Underneath the hood, `ping` issues ICMP Echo Requests and waits for Echo Replies.

<br />

### What's ICMP?

ICMP stands for Internet Control Message Protocol.  It's a layer 3 protocol that network devices use to tell each other stuff like:

<br />
> Hey, I know you wanted to talk to 192.168.0.5, but I can't find where they are right now

<br />

To understand this a bit more, let's take a step back and talk about the internet.  Whenever a network device talks to another, like when your computer talks to Google.com, it does so through a series of routers and gateways.

![Gateway diagram](/assets/images/posts/ping3.png)

Gateways usually talk with each other, but sometimes they talk with the source hosts as well.  They do this in order to report errors -- the language in which this reporting is done is ICMP.

There are several ICMP message types.  Ping uses the types `Echo Request` and `Echo Reply`.

<br />

### What are Echo Requests and Replies?

An `Echo Request` is a type of ICMP message.  A network device sends this as a "hello, are you there?" message to another device.

![Echo Request Diagram](/assets/images/posts/ping1.png)

According to ICMP's specification, the other device should respond with an `Echo Reply`.

![Echo Reply Diagram](/assets/images/posts/ping2.png)

<br />

### Diving Deeper

<br />

### Refs

* [RFC 792](https://datatracker.ietf.org/doc/html/rfc792)

