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

## Current Result and Reflections
### Planned to Achieve in the Proposal
<span style="color:green">*Done*</span>:
1. Build a functionally correct Fenwick tree with fine-grained locking
    - The code is significantly slower than the sequential version.
2. Determine appropriate lock granularity based on the read/write access pattern
    - No matter how the lock granularity is set, the program is still significantly slower than the sequential version.

<span style="color:yellow">*Ongoing*</span>:
1. Model-parallel Fenwick Tree: instead of parallelizing across operations, partition the internal array and assign each segment to a thread to reduce contention and improve locality
    - We implemented this approach and it has a nearly linear speedup with 2 threads, but the speedup starts to drop when more threads are launched.
    - With the special access patterns of the Fenwick tree, it might not be easy to achieve near-linear speedup even in low thread counts. We would still try to balance the workload in the next few weeks, but this goal might be listed as a "Hope to Achieve" instead of a "Plan to Achieve" right now. 

<span style="color:red">*TODO*</span>:

1. Benchmark and profile different parallelization strategies and workload distribution on multicore machines

### Hoped to Achieve in the Proposal
1. Achieve near-linear speedup even at high thread counts
    - It's even harder to achieve right now given the issue of workload imbalance mentioned above.
2. Explore alternative parallelization strategies: SIMD, CUDA
    - Instead of doing alternative parallelization strategies, it might be better to optimize the existing approaches first.

## New Goals
### Plan to Achieve
1. Analyze the reason for workload imbalance in the model-parallel implementation
2. Benchmark all implementations under varying batch sizes, array sizes, and thread counts
3. Analyze the impact of the query request percentage on performance
4. Profile and analyze cache coherence effects across all approaches

### Nice to Achieve
1. Achieve near-linear speedup at lower thread counts, especially when the internal array is sufficiently large

## Plan for Poster Session
1. Multiple graphs that show the experiment results for each parallel approach including:
    - Speedup vs thread counts
    - Speedup vs Fenwick tree size
    - L1/L3 Cache miss rate vs thread counts
    - Total number of L1/L3 cache miss rate vs thread counts
    - Query Request Percentage (LazySync) vs Baseline Performance
2. Some optimizations are implemented on the model-parallel Fenwick tree to eliminate the computation overhead for each thread. A graph might be shown to compare the speedup before and after the optimization.

**Last Updated**: Apr 15, 2025
