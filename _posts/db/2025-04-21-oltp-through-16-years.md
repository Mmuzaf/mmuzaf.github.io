---
layout: post
title: OLTP Through the Looking Glass 16 Years Later - Communication is the New Bottleneck
date: 2025-04-21
tags: [db]
---

Resources:

[OLTP Through the Looking Glass 16 Years Later - Communication is the New Bottleneck](https://vldb.org/cidrdb/papers/2025/p17-zhou.pdf)

** Overview**
- Bird-eye-view on the isolation in the OLTP systems: client-server isolation, no isolation, language isolation, 
process isolation, container isolation, VM isolation;
- Kernel Bypass: To leverage kernel bypass, call can be replaced the Linux networking stack (e.g. in VoltDB) with a user-space TCP/IP stack
(F-stack) using the DPDK library. So the idea is to bypass the kernel moving to the user space or from the user space to the kernel space 
(the latter is not trivial and have limitations e.g. the floater point calculations)


**Key Takeaways**
- Networking dominates: In simple workloads, messaging costs account for 52–68% of runtime; 
even with stored procedures, network overhead remains the "high pole in the tent".
- Stored procedures still win: Compared to client‑side transactions, stored procedures reduce round trips, 
lowering latency by up to 72% and boosting throughput by up to 2.1x for complex workloads (e.g., TPC‑C).
- Isolation trade‑offs: Stronger sandboxing (process, container, VM) inflates communication overhead by 31–90%; 
the best compromise uses shared‑memory IPC with polling, but even that incurs 31–66% extra cost versus no isolation;
- *Kernel bypass benefits & challenges*: User‑space TCP/IP stacks cut network CPU time from ~40% to ~10–20%, 
but integration complexity and maintenance burdens remain significant.


**Valuable Observations**
- Focus OLTP optimization on end‑to‑end messaging, not just DBMS core engines.
- Explore DB‑OS co‑design to develop lightweight IPC tailored for high‑performance isolation.
- Pursue user‑friendly kernel bypass frameworks that support full TCP/IP semantics without exclusive NIC access.
- Investigate stored‑procedure synthesis to marry low‑latency execution with client‑side flexibility stored procedure synthesis.

  