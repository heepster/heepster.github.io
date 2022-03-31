---
layout: post
title: "How does Ping work?"
category: networking
---

I've always had this vague notion that `ping` checks for connectivity between two hosts.  But what does that actually mean and how does it work?

<br />
Here's the TL;DR:

```
Ping issues ICMP Echo Requests and waits for Echo Replies.
```

<br />

### What's ICMP?

ICMP stands for Internet Control Message Protocol.  It's a layer 3 protocol that network devices use to tell each other stuff like:

<br />
> "Hey are you real and alive?"
  "Yup, I'm here and can hear you!"

or

> "Hey, I know you wanted to talk to 192.168.0.5, but I can't find where they are right now"

<br />

To understand this a bit more, let's take a step back and talk about the internet.  Whenever a network device talks to another, like when your computer talks to Google.com, it does so through a series of routers and gateways.  Packets originate from a source network device, or host, and are forwarded from gateway/router to gateway/router until they reach their destination.

![Gateway diagram](/assets/images/posts/ping3.png)

During this forwarding process, sometimes things go wrong.  For example, a packet might reach a gateway, only for it to realize that it can't keep forwarding that packet because the destination IP doesn't exist.  In cases like these, that gateway sends back an "error" -- in the form of an ICMP Destination Unreachable message.

<br />

### Other ICMP Message Types

There are other types of ICMP messages.  Ping uses the types `Echo Request` and `Echo Reply`, which are messages exchanged between two hosts (and not gateways/routers).

A network device sends an `Echo Request` as a "hello, are you there?" message to another device.

![Echo Request Diagram](/assets/images/posts/ping1.png)

According to ICMP's specification, the other device should respond with an `Echo Reply`.

![Echo Reply Diagram](/assets/images/posts/ping2.png)

<br />

### Diving Deeper

With all of these things in mind, let's ping something!

<br />

![Ping Terminal Screenshot](/assets/images/posts/ping-terminal.png)

<br />

We can see here that:
  * We ping'd `Google.com`
  * We ping'd twice (or looks like it)


<br />
Looking at [WireShark](https://www.wireshark.org/), we see that 4 packets were sent back and forth between our machine (`192.168.0.178`) and Google's server (`142.250.188.238`).

<br />

![Ping Wireshark Results](/assets/images/posts/ping-wireshark-result.png)

<br />

The first packet is an ICMP Echo Request packet from our machine to Google.  The second packet is an ICMP Echo Reply from Google to our machine.  The next two correspond to our second ping.

<br />

### No Time to Die

If you were wondering what `ttl` represents in `ping`'s output, the WireShark output sheds a bit of light on this.  Notice that in the first packet, the `ttl` is `64`.  In the reply packet, the `ttl` is `57`.  `ttl` stands for `Time to Live`, but in this context its better to think of it as "number of hops" -- in fact, ipv6 renames `ttl` to `hop_limit`.

Each time a gateway or router recieves an IP packet, it decrements the `ttl` field by `1` before forwarding it on.  Packets that have `ttl = 0` are dropped.

So, for our `ping`ing of Google, we know that we "hopped" `64 - 57` = `7` times.

<br />

### Refs

* [RFC 792](https://datatracker.ietf.org/doc/html/rfc792)

