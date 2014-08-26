---
layout: post
title: Offline first &sube; intermittent &sube; asynchronous
description:
modified: 2014-08-26
category: articles
tags:
image:
  feature: metabolic.png
  credit: Wikipedia
  creditlink: http://en.wikipedia.org/wiki/Metabolic_pathway#mediaviewer/File:Metabolism_790px.png
comments: true
share: true
---

This post is mostly a comment to the [offline-first manifesto](http://blog.hood.ie/2013/11/say-hello-to-offline-first/).
After careful consideration, I am pretty certain that speaking of offline-first
mobile/web apps is not that useful per se.
Indeed, these days apps are mostly consumed on wireless devices (laptops,
tablets, phones), so "offline" really happens. Strictly speaking, that
even happens to datacenters time to time. But, "offline" is probably a wrong
word for it. It is much more productive to speak about "intermittent
connectivity". Wireless connectivity *is* unstable just because of the physics.
There are device handovers, blind spots, mass gatherings, elevators and tonnels.
A device might be kind of "online", but it cannot fetch that data right now,
sorry. Unless we're considering military applications, "intermittent" reflects
that point much better.

The next step is to accept that intermittent connectivity is just a corner
case of the "asynchronous". When we enter an elevator, our app will receive
its API response one minute late. When a connection is "bad", the app may receive its response
ten seconds late. Is it that much different?
Whether we are formally "online" or "offline" does not
really matter. If the state changes before we get a response to our request,
then we *are* asynchronous. The RPC/HTTP world tries hard to conceal that fact, but
concealing only works that far.

Asynchrony is pretty much everywhere in this world and it affects everything.
In that regard, I recall a nice chat with a customer:

> C: You did a really good job optimizing the app last week! <br/>
> I: We did no optimization. By the way, where are you? <br/>
> C: I'm in New York. <br/>
> I: I see. We have our server there.

Mixing synchronous and asynchronous is like mixing beer and vodka: results are
painful. Whether we speak of "callback hell", "promises", or "operational
transformation", the underlying tensions are rooted in the fact that we chase
those two rabbits: synchronous logic and asynchronous communication.

Our current reality is massive amounts of users interacting in real time using
mobile devices. The process is served by massive amounts of servers also
interacting in real time. Everything is faulty, everything is distributed.
Hence, everything is asynchronous.
Reactive architectures, event buses, eventual consistency and all that new stuff
reflects one important fact.
We are on the road out of the Don Knuth's world of perfect cause-and-effect
chain-of-thought logic towards a new world that is more alike to biological
signaling pathways: efficient, massive, fault-tolerant and apparently chaotic
to an untrained eye.

Well then, what "offline first" is useful for? I think, it is a really good
criteria to evaluate how async-friendly you are. Very much like [fire drill](http://highscalability.com/blog/2010/12/28/netflix-continually-test-by-failing-servers-with-chaos-monke.html)
techniques, running your app offline tests how good you are at handling all the
zillion varieties of asynchrony simply by taking it to the extreme.
