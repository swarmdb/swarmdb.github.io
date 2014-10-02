---
layout: page
permalink: /about/
title: Swarm — reactive data sync lib
tags: [Jekyll, theme, simple, minimal, minimalism, responsive]
modified: 2013-09-13
---


Swarm is a reactive data sync library and middleware. Swarm synchronizes your
app\'s model automatically, in real time.

Why do we make Swarm?  Because new opportunities bring new challenges. Now,
having all that laptops, smartphones and tablets on WiFi/3G, we need handover
(aka continuity), real time sync and offline work. Those requirements stressed
classic request-response HTTP architectures leading to fix-on-a-fix stacks that
are far from perfection. We have built some apps like that.

Our dream is to develop distributed applications like good old local MVC apps,
by fully delegating the data caching/synchronization magic to a dedicated
layer. We want to deal with the data uniformly, no matter where it resides. We
believe, that CRDT is the only approach that allows to fully embrace the
reality of distributed data.

Swarm is a CRDT-based replicated model library (M of MVC) that keeps your data
correctly cached and synchronized in real time using any storage and transport
available.

