## The links of white papers, articles, and other resources that touch on the topic of the database links and key takeaways

Last updated: 2024-10-17

The post is updated regularly to include the latest resources and key takeaways and to remove the outdated ones. \
The resources are listed in the order of my personal investigation and interest in the topic.

---

#### Andy Pavlo — The official ten-year retrospective of NewSQL databases (2022)

Resource: https://www.youtube.com/watch?v=LwkS82zs65g

Key Takeaways:

- **Selling a new OLTP** distributed database startup **is hard** because it is a product of a high-risk. \
In comparison, the OLAP distributed database startup is a low-risk due to the impact of the running queries on the business 
(e.g. no one loses the revenue if the query is slow).
- 5 key points of the NewSQL databases [^1][^2] :
  - **Distributed transactions** ACID support for transactions
  - **SQL** (not NoSQL) as a primary interface for the database
  - **Scale-out** (not scale-up)
  - **Consistency** - non-locking concurrency control
  - **Performance** - high per-node performance
- **Shared-nothing architecture** is a key to the NewSQL databases, but it is not a panacea. In the opposite side, the 
shared-disk architecture is also viable for the OLTP databases (e.g. Google Spanner, NeonDB).
- **Middleware solutions** was a big trend in the 2010s, but they were not successful and **were squeezed out** by 
the cloud providers and the NewSQL databases.
- **Automation** of the database management **is a key** to the success of the NewSQL databases, whatever architecture 
they are based on for the next decade.

> The NewSQL Databases == Distributed databases with ACID transactions and SQL interface.

[^1] : [Stonebraker on NoSQL and Enterprises](https://dl.acm.org/doi/pdf/10.1145/1978542.1978546)
[^2] : [Stonebraker on Data Warehouses](https://dl.acm.org/doi/pdf/10.1145/1941487.1941491)

---
