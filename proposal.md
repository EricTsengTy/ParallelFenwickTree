---
layout: default
title: "Project Proposal"
permalink: "/proposal"
---

# Parallel Fenwick Tree 

**Team Members**: Tzu-Yen Tseng (tzuyent) & Jim Shao (chiatses)

## Project Web Page
> [https://erictsengty.github.io/ParallelFenwickTree](https://erictsengty.github.io/ParallelFenwickTree)

---

## Summary
We are implementing a parallelizable Fenwick Tree (Binary Indexed Tree) on CPU to enable concurrent read and write operations.

---

## Background
While segment trees and prefix sums have been widely studied in parallel computing, Fenwick trees remain relatively unexplored. As a lightweight data structure supporting range queries, a parallel Fenwick tree could offer performance benefits with lower overhead. A pseudocode of standard serial implementation is as follows:
```c++
class FenwickTree {
    int agg[n]; // Internal array

    // Add val to array[x]
    void add(int x, int val) {
        for (++x; x < n; x += x & -x) {
            agg[x] += val;
        }
    }

    // Get prefix sum of array[0..x-1]
    int sum(int x) {
        int res = 0;
        for (++x; x > 0; x -= x & -x) {
            res += agg[x];
        }
    }
    return res;
};
```

A straighforward parallelization approach is to use a fine-grained lock on each `agg[i]`:
```c++
class FenwickTree {
    int agg[n]; // Internal array
    lock_t locks[n];

    // Add val to array[x]
    void add(int x, int val) {
        for (++x; x < n; x += x & -x) {
            locks.lock();
            agg[x] += val;
            locks.unlock();
        }
    }

    // Get prefix sum of array[0..x-1]
    int sum(int x) {
        int res = 0;
        for (++x; x > 0; x -= x & -x) {
            locks.lock();
            res += agg[x];
            locks.unlock();
        }
    }
    return res;
};
```

Another simple alternative is to use atomic variables:
```c++
class FenwickTree {
    atomic<int> agg[n]; // Internal array

    // Add val to array[x]
    void add(int x, int val) {
        for (++x; x < n; x += x & -x) {
            agg[x] += val;
        }
    }

    // Get prefix sum of array[0..x-1]
    int sum(int x) {
        int res = 0;
        for (++x; x > 0; x -= x & -x) {
            res += agg[x];
        }
    }
    return res;
};
```

---

## The Challenges
- Managing synchronization between write/write or write/read operations
- Access patterns exhibit non-uniform contention; for example, some elements in the internal array are accessed significantly more frequently than others
- Each read/write operation accesses multiple, scattered locations in the internal array, leading to poor data locality under the naive parallel approach implementation
- We attempted to parallelize the Fenwick tree using both fine-grained locks and atomic operations as baselines, but both approaches performed worse than the sequential version, leading to a 2.0x slowdown..

---

## Resources
- Environment
    - GHC and PSC machines
- Code
    - [Fenwick Tree - Serial Version](https://cp-algorithms.com/data_structures/fenwick.html)
    - Sample code for microbenchmarking a range-query supported data structure
        - Haven't optained yet

---

## Goals and Deliverables
### Plan to Achieve
- Build a functionally correct Fenwick tree with atomic variables
- Build a functionally correct Fenwick tree with fine-grained locking
- Determine appropriate lock granularity based on the read/write access pattern
- Pipelining: instead of parallelizing across operations, partition the internal array and assign each segment to a thread to reduce contention and improve locality
- Benchmark and profile different parallelization strategies and workload distribution on multicore machines
- Given the high contention in the Fenwick tree, we aim to achieve near-linear speedup at lower thread counts, especially when the internal array is sufficiently large

### Hope to Achieve
- Achieve near-linear speedup even at high thread counts
- Explore alternative parallization strategies: SIMD, CUDA

---

## Platform Choice
- GHC, PSC (Linux)
    - GHC and PSC are chosen due to their high core counts, which align well with our focus on CPU-based parallel computing
- C++
    - C++ is selected due to its support of parallel programming libraries like OpenMP and Cilk, which we plan to leverage during our development

---

## Schedule

| **Date**      | **Goal** |
|---------------|----------|
| 4/1           | Build multi-versions of parallelizable Fenwick tree, focusing on parallelism across operations |
| 4/3           | Build the framework for microbenchmarking |
| 4/6           | Tune and finalize data-parallel approaches based on profiling results. |
| 4/8            | Develop and begin testing pipelining approaches |
| 4/15 *(Check-in)* | Finalize the pipelining approaches based on profiling results |
| 4/22          | Benchmark all approaches and compare the results |
| 4/29          | Complete the final report and poster |

**Last Updated**: March 26, 2025
