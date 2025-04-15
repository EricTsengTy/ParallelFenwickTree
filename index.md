---
layout: default
title: Home
---

# Parallel Fenwick Tree

**Team Members**: Tzu-Yen Tseng (tzuyent) & Jim Shao (chiatses)

Welcome to our project website! Below are links to our key pages:

- [Project Proposal]({{ site.baseurl }}/proposal)
- [Milestone Report]({{ site.baseurl }}/milestone)
- [Final Report]({{ site.baseurl }}/final-report)

We are implementing a parallelizable Fenwick Tree (Binary Indexed Tree) to enable concurrent read and write operations.

## Schedule

### Done

| **Date**      | **Task** |
|---------------|----------|
| 4/7           | Implemented sequential Fenwick tree |
|               | Built basic testing framework for Fenwick trees |
|               | Implemented coarse-grained and fine-grained locking versions |
| 4/8           | Developed basic model-parallel Fenwick tree |
|               | Enhanced testing framework with metrics for batch size, internal array size, etc. |
| 4/13          | Profiled model-parallel implementation and identified workload imbalance |

### Next

| **Date**      | **Goal** | **Who** |
|---------------|----------|----------|
| 4/16          | Analyze the reason of workload imbalance in the model-parallel implementation and explore possible solutions | Tzu-Yen Tseng |
| 4/18          | Run tests on PSC and evaluate performance with high thread counts | Jim Shao |
| 4/21          | Benchmark all implementations under varying batch sizes, array sizes, and thread counts | Tzu-Yen Tseng |
|               | Analyze the impact of the query request percentage on performance | Jim Shao |
| 4/23          | Profile and analyze cache coherence effects across all approaches | Tzu-Yen Tseng |
| 4/25          | Draft findings, summarize results, and finalize visualizations for the report/poster | All |
| 4/28          | Complete the final report and poster | All |