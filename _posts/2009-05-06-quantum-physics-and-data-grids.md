---
layout: post
title: Quantum Physics and Data Grids
category: articles
---

One of quantum physics' crazier notions is that two particles seem to communicate with each other instantly, even when they're billions of miles apart. Albert Einstein, arguing that nothing travels faster than light, dismissed this as impossible "spooky action at a distance."

The great man may have been wrong. A series of recent mind-bending laboratory experiments has given scientists an unprecedented peek behind the quantum veil, confirming that this realm is as mysterious as imagined.

<div align="center"><img src="/images/quantum1.jpeg" alt="quantum1" title="quantum1" width="500" /></div>
<div align="center"><img src="/images/quantum2.jpeg" alt="quantum2" title="quantum2" width="500" /></div>

Based off this theory, there has been several computer science related experiments, especially revolving around <a href="http://en.wikipedia.org/wiki/Quantum_computing">Quantum Computer</a> and different encryption algorithms that I won't get into in this blog post. What I would like to suggest is how Quantum Physics can revolutionize the area of (In Memory ;) ) Data Grids.

One of the main problems of Data Grids is the ability to replicate state changes from one instance to the other, especially when using WAN. This highly ties into Brewer's CAP Theorem, which states that:

> When designing distributed web services, there are three properties that are commonly desired: consistency, availability, and partition tolerance. It is impossible to achieve all three. In this note, we prove this conjecture in the asynchronous network model, and then discuss solutions to this dilemma in the partially synchronous model.

What I am suggesting is that once the ability (and it very close, there are already companies building highly secure computer systems using Quantum Physics), CAP theorem will no longer be applicable.

As a thought experiment, imagine that a photon has an up spin, and that represents binary 1 (I know, it can get much more advance than that, I am simplifying things). A down spin represent binary 0. Once photons are "entangled" (we bring up our data grid), and then we separate them (across the building or across the ocean, does not really matter), we can get "instantaneous replication". Once we change the state of one photon, the other will change its state instantaneously (only when we check its state, but that is when we really care about it ;) ). By exhibiting this behavior, we actually can get all three properties of CAP theorem. 

Imagine as well the ability to store a "local cache" of the data. Since the size of data that can be "store" is exponentially bigger than current technology, and the fact that "state change" is not bounded by current technology (no need for wires), most people can have most of the data locally most of the time (which in itself, is relative). Once local data is changed, there is no need for 2PC or something like that in order to update the master data. For all intent and purposes, we hold the master data :).

Quantum Physics is going to revolutionize the way we go about and use technology. What I talked about is just the tip of the iceberg, but I personally believe that once the technology starts maturing, the impact it will have on the world will dwarf the arrival of computers, the industrial revolution, or any other major event that occurred in our not so long history.

One of Einstein famous quotes regarding Quantum physics is "God doesn't play dice with the universe". I personally like better Neils Bohr, a big proponent of quantum uncertainty, rebuttal: "Quit telling God what to do."
