---
layout: default
title: "Project Proposal"
permalink: "/proposal"
---

# Parallel Fenwick Tree 

**Team Members**: Tzu-Yen Tseng (tzuyent) & Jim Shao (chiatses)

## URL
> [https://erictsengty.github.io/ParallelFenwickTree](https://erictsengty.github.io/ParallelFenwickTree)

---

## Summary
We are implementing a parallelizable Fenwick Tree (Binary Indexed Tree) to enable concurrent read and write operations.

---

## Background
While segment trees and prefix sums have been widely studied in parallel computing, Fenwick trees remain relatively unexplored. As a lightweight data structure supporting range queries, a parallel Fenwick tree could offer performance benefits with lower overhead.

---

## The Challenge
- Managing synchronization between write/write or write/read operations
- Access patterns exhibit non-uniform contention; for example, some elements in the internal array are accessed more frequently than others
- We attempted to parallelize the Fenwick tree using both the follwoing baseline approach and atomic operations, but both approaches underperformed the sequential version (2.0x slow down).

---

## Resources
- Build a parallelizable Fenwick tree from scratch and test on GHC and PSC machines for benchmarking the tree performance in parallel scenario.

---

## Goals and Deliverables
- Build a functionally correct Fenwick tree with fine-grained locking
- Determine appropriate lock granularity based on the read/write access pattern
- Benchmarking different parallel strategies and workload distribution on parallel machines

<!-- ### Plan to Achieve (minimum)

### Hope to Achieve (stretch) -->

---

## Platform Choice
- Linux
- C++

---

## Schedule

| Timeline      | Goal |
| --------      | -------- |
| 4/1           | Build multi-versions of parallelizable Fenwick tree |
| 4/8           | Benchmark strategy 1 |
| 4/15 (checkin)| Benchmark strategy 2 |
| 4/22          | Benchmark strategy 3 |
| 4/29          | Finish Final Report and Poster |

**Last Updated**: March 26, 2025
