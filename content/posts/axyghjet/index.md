---
title: CRDTs for Collaborative Writing
date: 2021-07-15
description: Introduction to CRDTs
---

## Init Journey

These past months I've been helping out with technical screen interviews on the job, the regular 1-hour meeting in which the candidate is given a code challenge and must provide a decent working solution by the end of it. 

Surprisingly enough, the place I work at does not provide access to any interview platform to assess the candidate's skills, leaving me in a tight spot.  From time to time, I have to request them to open the IDE of their choice and kindly share the screen, which I found invasive and likely to reduce the formality of the process.

Nowadays, there is a wide variety of paid services online such as CoderPad, CodeInterview, HackerRank, CoderByte; Just to mention a few, and all of them offer robust capabilities.

Similarly, on the side of free software, we have etherpad, collabedit, and Plunker. These last ones have been an experience-saver when the candidate cannot share his screen for any reason.

This situation got me intrigued and oddly inspired.

How do all these real-time collaborative text platforms operate? What is the engineering behind it?  Which type of technical challenge do they face, and which approaches do they follow? Plenty of questions, right? So, it is time to start research on the foundations.


## Collaborative Text: How does it work?

Collaborative text web applications are asynchronous, fully powered by distributed systems, and challenged by their complexity. Consistency in the operations executed by the users takes the primary seat, while they are directly affected by the latency between the nodes.


If two users write, update or delete the same word in the document. Which change is the right one? Is it the last one? Is it the first one? How do we handle this edit conflict? 

In an article published in 2011 by _Marc Shapiro, Nuno Preguiça, Carlos Baquero, and Marek Zawirski_ named **"A comprehensive study of Convergent and Commutative Replicated Data Types."**[^1] they propose the design of shared data types capable of conflict-free eventual consistency.

Quoting: 

> Eventual consistency aims to ensure that replicas of some mutable shared object converge without foreground synchronisation. Previous approaches to eventual consistency are ad-hoc and error-prone. We study a principled approach: to base the design of shared data types on some simple formal conditions that are sufficient to guarantee eventual consistency. We call these types Convergent or Commutative Replicated Data Types (CRDTs). 

This next infographic is meant to provide you an understable glimpse of eventual consitency.

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

## Building basic CRDTs

### Counters 

The most basic of the CRDTs, a primitive that lives within the replicas exposing a set of basic operations:

- **Increment/decrement** the value of the counter.
- **Update** the value of the counter.
- **Query** the value of the counter.

The value of the primitive must converge towards the global number of increments minus the number of decrements. These are some of the defined CRDTs counters [^1]:

- Grow-only Counter (G-Counter):  state-based counter for positive increments only.
- Positive-Negative Counter (PN-Counter): state-based counter with a double registry for positive and negative increments.
- Non-negative Counter: similar to PN-Counter with the restriction of not beign able to generate more decrements than the current originated increments.

To provide a more thorough view of one the counters mentioned above, we picked G-Counter to further explain its operations.

### Grow-only Counter (G-Counter)

G-Counter is a state-based CRDT or CvRDT for positive increments only. As mentioned above it exposes a set of basic operations: *init, increment, value, merge and compare.*

![G-COunter OPs 1](/images/excalidraws/g-counter-ops-pt1.png)
#### G-Counter OPS: init, increment and value.

![G-COunter OPs 2](/images/excalidraws/g-counter-ops-pt2.png)
#### G-Counter OPS: merge and compare.

To further visualize a GCounter, we can easily create a class named `GCounter` that implements each of the methods explained above.

``` java {linenos=table,linenostart=1}
package com.squaredcow.crdts;

import lombok.Data;

import java.util.Arrays;

@Data
public final class GCounter implements Counter<Integer> {

    private Integer registryId;
    private Integer registrySize;
    private Integer[] innerRegistry;

    public GCounter(Integer registryId, Integer registrySize) {
        this.registryId = registryId;
        this.registrySize = registrySize;
        this.innerRegistry =  new Integer[registrySize];
    }

    @Override
    public GCounter init() {
        Arrays.fill(this.innerRegistry, 0);
        return this;
    }

    @Override
    public GCounter increment() {
        Integer id = this.registryId;
        System.out.println("Increasing by 1 G-Counter for id=" + id);
        this.innerRegistry[id] = this.innerRegistry[id] + 1;
        return this;
    }


    @Override
    public Integer value() {
        Integer result = 0;
        for(Integer counter : innerRegistry) {
            result += counter;
        }
        return result;
    }

    public Boolean compare(GCounter x, GCounter y) {
        for(Integer n : x.getInnerRegistry()) {
            if(x.getInnerRegistry()[n] <= y.getInnerRegistry()[n]) {
                return true;
            }
        }
        return false;
    }

    public Integer[] merge(GCounter x, GCounter y) {
        Integer[] mergedRegistry = new Integer[registrySize];
        for(int i = 0; i < x.getInnerRegistry().length; i ++) {
            Integer vx = x.getInnerRegistry()[i];
            Integer vy = y.getInnerRegistry()[i];
            mergedRegistry[i] = Math.max(vx, vy);
        }
        return mergedRegistry;
    }
}
```

##  Next steps

This is an initial dive into the CRDTs structures themselves, we will eventually go deeper to construct a cluster of replicas that operate with them. We still need to go first through the rest of the structures mentioned, such as: *registers, sets and graphs*, but let's have those on a second entry on this blog series.

With nothing left to add, thank you for reading all the way through, until next entry.

-- squaredcow

[^1]: Paper: https://hal.inria.fr/inria-00555588/document