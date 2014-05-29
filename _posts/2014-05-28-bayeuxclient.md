---
layout: post
title: BayeuxClient
banner: /assets/images/posts/post2.jpg
small_banner: /assets/images/posts/post2_small.jpg
thumb: /assets/images/posts/post2_thumb.jpg
---
A couple days late with the news, but I have completed v0.1.0 of my first Objective-C library: [BayeuxClient](https://github.com/bmatcuk/BayeuxClient).

BayeuxClient is a client library that implements the [Bayeux Protocol](http://svn.cometd.org/trunk/bayeux/bayeux.html): a protocol designed to transport asynchronous messages between a web server and clients. One of the most common server-side implementations of this protocol is [Faye](http://faye.jcoglan.com/).

At this point, BayeuxClient only supports communication via WebSockets, implemented using the excellent [SocketRocket](https://github.com/square/SocketRocket) library. However, one of the major design goals of the Bayeux Protocol is the ability to swap out pieces. Locking into a single "transport protocol" (ie, WebSockets) kind of goes against this. So, in the future, if time allows, I may break out the WebSocket transport layer and implement support for other transports (HTTP long-polling, for example). We'll see =)

In any event, this library is already being used in at least two production applications: one for OSX and the other for iOS. So, I feel fairly confident that it is "production ready". It works on both OSX v10.7 ("Lion") or higher, and iOS 5.0+. Though, it must be compiled with ARC support. It's also available via [cocoapods](http://cocoapods.org/) to make adding it to your project easier.