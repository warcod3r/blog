---
title: CRDTs for Collaborative Writing
date: 2021-07-15
description: Introduction to CRDTs
---

## Init Journey

These past months I've been helping out with technical screen interviews on the job, the regular 1-hour meeting in which the candidate is given a code challenge and must provide a decent working solution by the end of it. 

Surprisingly enough, the place I work at does not provide access to any interview platform to assess the candidate's skills, leaving me in a tight spot.  From time to time, I have to request them to open the IDE of their choice and kindly share the screen, which I found invasive and likely to reduce the formality of the process.

Nowadays, there is a wide variety of paid services online such as CoderPad, CodeInterview, HackerRank, CoderByte; Just to mention a few, and all of them with robust capabilities.

Similarly, on the side of free software, we have etherpad, collabedit, and Plunker. These last ones have been an experience-saver when the candidate cannot share his screen for any reason.

This situation got me intrigued and oddly inspired.

How do all these real-time collaborative text platforms operate? What is the engineering behind it?  Which type of technical challenge do they face, and which approaches do they follow? Plenty of questions, right? So, it is time to start research on the foundations.


## Collaborative Text: How does it work?

Collaborative text web applications are asynchronous, fully powered by distributed systems, and challenged by their complexity. Consistency in the operations executed by the users takes the primordial seat.

If two users write, update or delete the same word in the document. Which change is the right one? Is it the last one? IS it the first one? How do we handle this edit conflict? The operations are directly affected by the latency between the nodes.

In an article published in 2011 by _Marc Shapiro, Nuno Preguiça, Carlos Baquero, and Marek Zawirski_ named **"A comprehensive study of Convergent and Commutative Replicated Data Types."**[^1] they propose the design of shared data types capable of conflict-free eventual consistency.

Quoting a fragment of the Abstract of given research: 

> Eventual consistency aims to ensure that replicas of some mutable shared object converge without foreground synchronisation. Previous approaches to eventual consistency are ad-hoc and error-prone. We study a principled approach: to base the design of shared data types on some simple formal conditions that are sufficient to guarantee eventual consistency. We call these types Convergent or Commutative Replicated Data Types(CRDTs). 

This next infografic is meant to provide you an understable glimpse of eventual consitency.

![The AP of CAP](/images/excalidraws/the_ap_of_cap.png)
Powered by Excalidraw.

## Diving into CRDTs

### What is a CRDT?


We propose the concept of a convergent or commutative replicated data type (CRDT), forwhich some simple mathematical properties ensure eventual consistency. 

, 
- Replicas of any CRDT converge to a common state that is equivalent to some correct sequential execution. 

- As a CRDT requires no synchronisation, an update executes immediately, unaffected by network latency,faults, or disconnection. 

- It is extremely scalable and is fault-tolerant, and does not requiremuch mechanism. 
 
 Application areas may include computation in delay-tolerant networks,latency tolerance in wide-area networks, disconnected operation, churn-tolerant peer-to-peercomputing, data aggregation, and partition-tolerant cloud computing.

A data-structure designed for distributed computing meant to be used in distributed storages and multi-user applications.

# Building basic CRDTs

## Counters 

The most basic of the CRDTs, a primitive that lives within the replicas exposing a set of basic operations:

- **increment/decrement** the value of the counter.
- **update** the value of the counter.
- **query** the value of the counter.

The value must converge towards the global number of increments minus the number of decrements. 

### Grow-only Counter (G-Counter)

G-Counter is a state-based CRDT or CvRDT.

``` java
pacakge com.expediagroup.*;

public class GCounter<Integer> implements Counter { 
   
    private Integer[] 
    payload integer[n] P
    initial [0,0,...,0]

    @Override
    public void increment() {
      Integer g = myId()
      P[g] := P[g] + 1
    }


    @Override
    public Integer value() {
        Integer result = 0;
        for(Integer counter : clusterCounters) {
            result += counter;
        }
        return result;
    }
    
    @Override
    public boolean compare (X, Y) {
        let b = (∀i ∈ [0, n - 1] : X.P[i] ≤ Y.P[i])
    }
    
    public merge (X, Y) : payload Z
    let ∀i ∈ [0, n - 1] : Z.P[i] = max(X.P[i], Y.P[i])
}
```

## Registries

A register is a memory cell storing an opaque atom or object (noted type X here after). 

That supports to primary operations:
- assign: to update the registry value.
- value: to query the registry.

Non-concurrent assigns preserve sequential semantics: the later one overwrites the earlier one. 

Unless safeguards are taken, concurrent updates do not commute; two major approaches are:
- Last-Writer-Wins Registry  or LWW-Register that one takes precedence over the other 
- Multi-Value Register or MV-Register that both are retained 


[^1]: Paper: https://hal.inria.fr/inria-00555588/document