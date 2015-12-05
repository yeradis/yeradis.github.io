---
layout: post
title: Some I/O Scheduler types
date: 2011-8-27
tags: 
comments: true
permalink:
share: true
---

Completely Fair Queuing—elevator=cfq (default)
Deadline—elevator=deadline
NOOP—elevator=noop
Anticipatory—elevator=as

The Completely Fair Queuing (CFQ) scheduler,as the name implies, CFQ maintains a scalable per-process I/O queue and attempts to distribute the available I/O bandwidth equally among all I/O requests. CFQ is well suited for mid-to-large multi-processor systems and for systems which require balanced I/O performance over multiple LUNs and I/O controllers.

The Deadline elevator uses a deadline algorithm to minimize I/O latency for a given I/O request. The scheduler provides near real-time behavior and uses a round robin policy to attempt to be fair among multiple I/O requests and to avoid process starvation. Using five I/O queues, this scheduler will aggressively re-order requests to improve I/O performance.

The NOOP scheduler is a simple FIFO queue and uses the minimal amount of CPU/instructions per I/O to accomplish the basic merging and sorting functionality to complete the I/O. It assumes performance of the I/O has been or will be optimized at the block device (memory-disk) or with an intelligent HBA or externally attached controller.

The Anticipatory elevator introduces a controlled delay before dispatching the I/O to attempt to aggregate and/or re-order requests improving locality and reducing disk seek operations. This algorithm is intended to optimize systems with small or slow disk subsystems. One artifact of using the AS scheduler can be higher I/O latency.

"));