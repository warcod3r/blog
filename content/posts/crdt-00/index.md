---
title: CRDTs for Collaborative Writing
date: 2021-06-01
description: Introduction to CRDTs
draft: true
---

## Init Journey

These past months I've been helping out with technical screen interviews on the job, the regular 1-hour meeting in which the candidate is given a code challenge and must provide a decent working solution by the end of it. That leaves me in a tight spot from time to time with the candidates.

Nowadays, there is a wide variety of paid services online such as CoderPad, CodeInterview, HackerRank, CoderByte; Just to mention a few, and all of them with robust capabilities.

Similarly, on the side of Free Software, we have etherpad, collabedit, and Plunker. Particularly, collabedit has been a life-saver in times in which the candidate was looking forward to using one of the before-mentioned tools.

This situation got me intrigued and inspired to kickstart a side project about an open-source real-time collaborative text platform for technical screening interviews. Quite ambitious, right? That's why I will shrink the scope and increase it with time, but first, let us start with the foundations.

## Collaborative Text: How does it work?

Collaborative text web applications are asynchronous, fully powered by distributed systems, and challenged by their complexity. Consistency in the operations executed by the users takes the primordial seat.

If two users write, update or delete the same word in the document. Which change is the right one? Is it the last one? How do we handle this edit conflict? The operations are directly affected by the latency between the nodes.

In an article published in 2011 by _Marc Shapiro, Nuno Preguiça, Carlos Baquero, and Marek Zawirski_ named **"A comprehensive study of Convergent and Commutative Replicated Data Types."**[^1] they propose the design of shared data types capable of conflict-free eventual consistency.

Quoting a fragment of the Abstract of given research: 

> Eventual consistency aims to ensure that replicas of some mutable shared object converge without foreground synchronisation. Previous approaches to eventual consistency are ad-hoc and error-prone. We study a principled approach: to base the design of shared data types on some simple formal conditions that are sufficient to guarantee eventual consistency. We call these types Convergent or Commutative Replicated Data Types(CRDTs). 

This next infografic is meant to provide you an understable glimpse of eventual consitency.

![The AP of CAP](/images/excalidraws/the_ap_of_cap.png)
Powered by Excalidraw.

## Diving into CRDTs

### What is a CRDT?


We propose the concept of a convergent or commutative replicated data type (CRDT), forwhich some simple mathematical properties ensure eventual consistency. 

Provably, 
- Replicas of any CRDT converge to a common state that is equivalent to some correct sequential execution. 

- As a CRDT requires no synchronisation, an update executes immediately, unaffected by network latency,faults, or disconnection. 

- It is extremely scalable and is fault-tolerant, and does not requiremuch mechanism. 
 
 Application areas may include computation in delay-tolerant networks,latency tolerance in wide-area networks, disconnected operation, churn-tolerant peer-to-peercomputing, data aggregation, and partition-tolerant cloud computing.

A data-structure designed for distributed computing meant to be used in distributed storages and multi-user applications.


[^1]: Paper: https://hal.inria.fr/inria-00555588/document