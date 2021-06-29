---
title: CRDTs for Collaborative Writing
date: 2021-06-01
description: Introduction to CRDTs
draft: true
---

# CRDTs: Conflict-free replicated data types

## Initial motivations

These past months I've been helping out with technical interviews on the job, the regular 1-hour meeting in which the candidate is given a code challenge and must provide a working solution by the end of it.

Believe it or not, the company I work for has +400 local employees, at least half of them engineers, and curious enough they do not pay any licensing for online technical interview platforms.  

There is a wide variety of paid services online. We have, CoderPad, CodeInterview, HackerRank, CoderByte to mention a few. But, on the side of free software, we have etherpad, collabedit and Plunker.

These last ones serve as an inspiration for what comes next on this series of building a collaborative text platform for technical screen interviews. 

## Collaborative Text: How does it work?

Collaborative text web applications are asynchronous, fully powered by distributed systems, and challenged by their complexity. Consistency in the operations executed by the users takes the primordial seat.

If two users write, update or delete the same word in the document. Which change is the right one? Is it the last one? How do we handle this edit conflict? The operations are directly affected by the latency between the nodes.

An article published in 2011 by Marc Shapiro, Nuno Preguiça, Carlos Baquero, and Marek Zawirski named "A comprehensive study of Convergent and Commutative Replicated Data Types"; They propose the design of shared data types capable of conflict-free eventual consistency.

Quoting the abstract: 

> We call these types Convergent or Commutative Replicated Data Types(CRDTs). This paper formalizes asynchronous object replication, either state-based or operation-based, and provides a sufficient condition appropriate for each case. It describes several useful CRDTs, including container data types supporting both "add" and "remove" operations with clean semantics, and more complex types such as graphs, monotonic DAGs, and sequences. It discusses some properties needed to implement non-trivial CRDTs.

## Diving into CRDTs

