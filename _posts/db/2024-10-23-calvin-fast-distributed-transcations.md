---
layout: post
title: Calvin — fast distributed transactions for partitioned database systems
date: 2024-10-23
tags: [db]
---

Resource:

https://dl.acm.org/doi/10.1145/2213836.2213838

Thoughts:

- Move as much work as possible earlier in the transaction to avoid the bottleneck of the sequencer and **out of the
  transaction boundaries**.
- **Artificial delays to enforce tx order.** Nondeterministic execution of transactions in a distributed databases can arbitrarily reorder the transactions maximizing
  the resource utilization, deterministic execution is a key concept in the Calvin database system, which respects the
  **sequencer's order** of the transactions, but introduces **artificial delays** for the scheduler to enforce the order.
- **Slow-storage and network delays handle the same.** Calvin's transaction throughput is unaffected by the presence of transactions that access slow storage nodes or
  that are access remote data partitions via the network.
- **Predict latencies in order to enforce deterministic execution.** The Calvin system uses the **predictive latency**
  to enforce the deterministic execution of the transactions both for the local and remote transactions. The sequencer
  must track which keys are in-memory and which are on-disk on all storage nodes to predict the latencies.

Key Takeaways:

- The sensitivity of the overall system throughput depends on two factors:
    - **The number of machines** in the system as for every multi partition transaction the system adds an overhead of:
      serealizing and deserealizing read requests, context switching between transaction to wait for the responses, setting
      up, executing and cleaning up the transactions context.
    - **The level of contention**. The higher the contention the more likely each machine's slowdown will affect the overall
      system throughput as it cause the other machines to slow down their transaction processing as well. 
  
