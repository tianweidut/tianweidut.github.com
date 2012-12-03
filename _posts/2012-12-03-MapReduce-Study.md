---
date: 2012-12-03 09:00:00
layout: post
title: MapReduce 学习笔记
categories:
    - Tech
tags: 
    - Hadoop
    - MapReduce
    - Cookbook
comments: yes
---

# Background
1. "Big Data" is a fact of the world, therefor an issue that real-world systems must grapple with.
    * Gathering, analyzing, monitoring, filtering, searching, or organizing web content.
2. More data translates into more effecitve algorightms, and thus it makes sense to take advantage of 
the plentiful amounts of data that surround us.
    * "web-scale" processing is patically synonymous with data-intensive processing.

# MapReduce
## Basics
### Problem
1. Divide and conquer
    * A large Problem into smaller sub-problems(which are independent).
    * In parallel by different workers
        * threads in a processor core
        * cores in a multi-core processor  
        * multiple processors in a machine
        * many machines in a cluster 
    **Intermediate results from each individual worker are then combined to yield the final output.**
    * Issues:
        * How do we break up a large problem into smaller tasks? 
        * How do we decomprose the problem so that the smaller tasks can be executed in parallel?
        * How do we assign tasks to workers distributed across a potentially large number of machines?
        * How do we ensure that the workers get the data they need?
        * How do we coordinate synchronization among the different workers?
        * How do we share partial results from one worker that is needed by another?
        * How do we accomplish all of the above in the face of software errors and hardware faults?

2. Hadoop Key Points:
    * It provides an abstraction that hide many system-level details from the programmer.
    * Move code and small data to big data
(to be continued...)
        



