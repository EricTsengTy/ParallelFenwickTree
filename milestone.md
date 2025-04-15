---
layout: default
title: "Milestone Report"
permalink: "/milestone"
---

# Milestone Report

**Team Members**: Tzu-Yen Tseng (tzuyent) & Jim Shao (chiatses)

## Project Web Page
> [https://erictsengty.github.io/ParallelFenwickTree](https://erictsengty.github.io/ParallelFenwickTree)

---

## Summary
We have implemented a sequential Fenwick tree as a baseline. Then we developed several parallelizable Fenwick trees.
1. Fine-grained locking Fenwick Tree: each element in the Fenwick Tree has a lock. This implementation performs 10x worse than the baseline.
2. Model-Parallel Fenwick Tree: split the internal array into several segments, which is assigned to each thread. Each thread owns a part of the Fenwick Tree to reduce the cache coherency traffic.
3. *LazySync* Fenwick Tree: separate each write to all processors and make sure all writes are performed before the query request. This method can beat the baseline when the query request is low (contains only 0.1% of the total workload).

## Thoughts
The progress of the goals and deliverables listed in our proposal will be described here.

### Plan to Achieve in the Proposal
- Build a functionally correct Fenwick tree with fine-grained locking
    - This approach was implemented as the most basic parallelization appraoch. We do not expect this approach to scale really well as we already had a draft version beforehand. With the fine-grained locking, the code is significantly slower than the sequential version.
- Determine appropriate lock granularity based on the read/write access pattern
    - We implemented a Fenwick tree where each lock might cover multiple elements in the internal array. However, no matter how the lock granularity is set, the program is still significantly slower than the sequential version.
- Pipelining: instead of parallelizing across operations, partition the internal array and assign each segment to a thread to reduce contention and improve locality
    - This approach was implemented and it does reduce the contention just as we expected in our proposal. This version has a nearly linear speedup with 2 threads, but the speedup starts to drop when more threads are launched. Also, instead of "pipelineing", now we prefer to refer to this approach as "model-parallel Fenwick tree" as the data doesn't actually need to move from one thread to another in the underdying implementation.
- Benchmark and profile different parallelization strategies and workload distribution on multicore machines
    - This hasn't been done yet, but will be done in the next few weeks.
- Given the high contention in the Fenwick tree, we aim to achieve near-linear speedup at lower thread counts, especially when the internal array is sufficiently large
    - In the model-parallel Fenwick tree implementation, we noticed that there is a workload imbalance among the threads. With the special access patterns of the Fenwick tree, it might not be easy to achieve near-linear speedup even in low thread counts. We would still try to balance the workload in the next few weeks, but this goal might be listed as a "Hope to Achieve" instead of a "Plan to Achieve" right now.


### Hope to Achieve in the Proposal

- Achieve near-linear speedup even at high thread counts
    - It's even harder to achieve right now given the issue of workload imbalance mentioned above.
- Explore alternative parallelization strategies: SIMD, CUDA
    - Instead of doing alternative parallelization strategies, it might be better to optimize the existing approaches. 

## New Goals
### Plan to Achieve
- Analyze the reason for workload imbalance in the model-parallel implementation
- Benchmark all implementations under varying batch sizes, array sizes, and thread counts
- Analyze the impact of the query request percentage on performance
- Profile and analyze cache coherence effects across all approaches

### Nice to Achieve
- Achieve near-linear speedup at lower thread counts, especially when the internal array is sufficiently large

## Plan for Poster Session
- Multiple graphs that show the experiment results for each parallel approach including:
    - Speedup vs thread counts
    - L1/L3 Cache miss rate vs thread counts
    - Total number of L1/L3 cache miss rate vs thread counts
- Some optimizations are implemented on the model-parallel Fenwick tree to eliminate the computation overhead for each thread. A graph might be shown to compare the speedup before and after the optimization. 

**Last Updated**: Apr 14, 2025
