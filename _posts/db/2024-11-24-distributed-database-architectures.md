---
layout: post
title: Distributed Database Architectures + DataStax JVector Talk (CMU Intro to Database Systems)
date: 2024-11-24
tags: [db]
---

Resources:

[#22 - Distributed Database Architectures + DataStax JVector Talk (CMU Intro to Database Systems)](https://www.youtube.com/watch?v=o7bjRUcDMx0)

**Key Takeaways**

From my point of view, this is an additional resource to polish the knowledge you already might have
about the distributed databases and their architectures. The most interesting part is about _JVector_ part and
is outlined below:

- Basic overview of the distributed architectures: **Shared-Nothing**, **Shared-Disk**, **Shared-Memory**. \
  Raw and limited examples of the real database engines that are based on these architectures.
- Basic knowledge about partitioning **vertical** and **horizontal**, consistent hashing,
  along with the description of their purpose.
- **Watch Jonathan Ellis' part** where he describes the **JVector** and white papers behind it:
    - [Efficient and robust approximate nearest neighbor search using Hierarchical Navigable Small World graphs](https://arxiv.org/abs/1603.09320)
    - [Understanding Hierarchical Navigable Small Worlds (HNSW)](https://www.datastax.com/guides/hierarchical-navigable-small-worlds)
    - [LM-DiskANN: Low Memory Footprint in Disk-Native Dynamic Graph-Based ANN Indexing (2019)](https://cse.unl.edu/~yu/homepage/publications/paper/2023.LM-DiskANN-Low%20Memory%20Footprint%20in%20Disk-Native%20Dynamic%20Graph-Based%20ANN%20Indexing.pdf)
    - Quicker ADC (Asymmetric Distance Computation) for the ANN search.
    - Product Quantization (PQ) for the ANN search.
    - It was notices that **JVector** implementations with **GC-based languages like Java are more efficient**
      than the C++ implementations due to the garbage collection and memory management. \
      _Would be interesting to investigate the reasons behind this statement._