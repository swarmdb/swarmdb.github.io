---
layout: post
title: An object as an event stream
description:
modified: 2015-10-03
category: articles
tags:
image:
    feature: day_and_night.jpg
    credit: M.C.Escher
comments: true
share: true
---

Objects, events and streams are popular abstractions all around Computer Science. In Swarm, all three concepts kind of morph, so I I feel the need to explain how Swarm became event-, stream- and object- oriented at the same time.

It definitely makes sense to start with definitions. By an *object* we mean a readable/writeable OOP-style object. The only addition is that it may have multiple *replicas* that need to stay in sync.  That adds a secondary requirement: object's methods must decompose into syncable CRDT ops, more on that later.

Swarm replicates data with the granularity of a single object. That is different from e.g. database replication, where the entire dataset is mirrored over. As we deal with client-side replicas, that was not an option. We see Swarm's object-centricity as an advantage over some other real-time sync technologies as the classic Entity-Relationship approach is well understood and maps nicely to SQL, noSQL, JSON, XML and, basically, everything else.

By an *event* we mean a state change event, such that:

* if it changes no state, it is no "event" and
* every state change is an event (or multiple events).

In fact, we use *event* almost synonymously with *operation* and, to a large degree, *method*. Event is an "arrow" on the object's state diagram. We are not discussing UI or IO events here, although in most cases those can be roughly mapped to state change events, or even mapped 1:1 in some simpler cases.
For clarity, let's call that operation-event-method an *op*.

*Stream* is a nice and ancient abstraction that allows either to write or to read data sequentially. Once we subscribe to an object, we receive a stream of state change events. That is different from the most popular pub-sub "channel" abstraction, where various events are dumped on a common *bus*, but there is no direct relation between events and state, and the bus is not a domain model object. Per-object granularity of event subscription fits reactive architectures much much better. Local state-change events are very popular in MVC architectures; Swarm extends that to distributed systems.

Physicists call it "dualism" that a particle and a wave are different projections of the same entity. So, Swarm has dualism where an object and an event stream are projections of the core principle. That dualism defines the architecture and further on extends to UI and IO behavior patterns. That is almost religious.

![What Mickey means]({{ site.url }}/images/mickey3nity.png)

A Swarm object is a stream of state change events, like this:

![An object is an event stream]({{ site.url }}/images/streams.svg)

A replica of an object produces a linear stream of state change events. That stream is listened another replica; operations are applied, states are synchronized. That is in line with the master-slave replication scheme (like MySQL or MongoDB is using).

![Master-slave replication]({{ site.url }}/images/streams-slave.svg)

Master-slave replication is inherently one-way as the master does linearization of writes. That is at odds with decentralized function in its very general sense.
Our objective is to let replicas work offline and under intermittent connectivity. Real-time communication, in general, demands latency independence. Even more generally, Swarm is built to function well in an asynchronous environment, which every distributed system actually is. That is why Swarm employs Commutative Replicated Data Types (CRDT).

![CRDT definition]({{ site.url }}/images/crdt.png)

CRDT to master-slave is what git is to CVS. Every argument on why git is not centralized is equally applicable to Swarm.
With CRDT, linearization is not needed, so every replica may send mutation events to other replicas.

![CRDT replication]({{ site.url }}/images/streams-CRDT.svg)

In Lamport's terms, our object replica is actually a *process*, as it sends/receives *messages* (ops) to/from other replicas asynchronously to sync the state. *Messages* are marked with Lamport timestamps (like `!local_time+process_id`, see a [post on timestamps][lamport]). Thus, the state of a replica can be described by a version vector. Lamport's model is not needed much in the master-slave model as all the changes come from a single source. In the distributed model, it is critical for understanding. Lamport's vocabulary is very popular in the CRDT literature.

[lamport]: http://swarmjs.github.io/articles/lamport/

So we may see an object's replica as a stream of state change events and those events may come in slightly different orders at different replicas. Eventually, all replicas get all the events, so their states converge. Still, the correct understanding of a Swarm *object* is more like "a Platonic ideal". Practically, we can read some particular replica, not an "object" itself. We may understand an object as a complete *swarm* of all its replicas, once it converges.

Further on, academic literature tends to distinct state vs op-based CRDT types. Another common assumption is that replicas exchange an uninterrupted flow of updates. Practically, that uninterrupted flow is an expensive and inconvenient abstraction. Intermittent connectivity is quite common with mobile devices. Some objects may no longer be needed but we want a (maybe stale) replica to remain in the local cache. Finally, a user may always close his/her laptop and we have no idea how soon it will get back online. Hence, interrupting the event stream is a practical necessity.

The straightforward solution of machinegunning missed events on reconnection is grossly inefficient. Swarm's underlying solution is a stream initiation handshake exchanging full or partial object states (snapshots or patches), so replicas may proceed with their continuous event streams further on. Actually, the logo of the project depicts a handshake.

![Handshakes and replication]({{site.url}}/images/streams-gaps.svg)

So, any *object* is actually a *replica*. Any state change is an *event* or an *operation*, depending on whether you read or write it. All events on a replica form a *stream*. Finally, all those are different projections of the same ideal entity named "a replicated object".

I hope, this post explains Swarm's core abstractions to a  sufficient degree.

P.S. I'd like to acknowledge @apronchenkov for useful feedback.
