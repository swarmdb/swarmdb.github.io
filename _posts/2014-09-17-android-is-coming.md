---
layout: post
title: Java version is in the works
description:
modified: 2014-09-17
category: articles
tags:
comments: true
share: true
---

Swarm is a generic CRDT-based model for data synchronization that works in real
time. Swarm is also highly tolerant to asynchronous environments, including the
most extreme case of it: offline work. As an algorithm, Swarm can be
implemented in any programming language. It was a little bit annoying that we
only had a JavaScript version so far. Not anymore.


Finally, the Java version is making its first steps both on server side and
Android devices:

<iframe width="560" height="315" src="//www.youtube.com/embed/KK1AjVvAfE8" frameborder="0" allowfullscreen></iframe>

That brings us one step closer to the vision of replicated model as the new
[hourglass waist][waist] of distributed app architectures.

![Swarm: deployment]({{ site.url}}/images/swarm-moscowjs-deployment.png)

Replicated model everywhere!

[waist]: http://www.gtresearchnews.gatech.edu/hourglass-internet-architecture/
