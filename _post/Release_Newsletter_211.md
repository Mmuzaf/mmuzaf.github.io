# Apache Ignite 2.11: Stabilization First

The new [Apache Ignite](https://ignite.apache.org/) 2.11 was released on September 17, 2021. It can be considered to a greater 
extent as a stabilization release that closes more technical debts of the internal architecture and bugs. Out of more than 
200 completed tasks, 120 are bug fixes. However, some valuable improvements still exist, so let's take a quick look at them together.


## Thin Clients

Partition awareness is enabled by default in the 2.11 release and allows the thin client to send query requests directly to the 
node that owns the queried data. Without partition awareness, an application executes all queries and operations via 
a single server node that acts as a proxy for the incoming requests.

The support of [Continuous Queries](https://ignite.apache.org/docs/latest/thin-clients/java-thin-client#cache-entry-listening) 
are added to the java thin client. For the other supported features, you may check -
the [List of Thin Client Features](https://cwiki.apache.org/confluence/display/IGNITE/Thin+clients+features).


## Cellular-clusters Deployment

The Apache Ignite internals has the so-called the `switch` (a part of Partition Map Exchange) process that is used to perform
atomic execution of cluster-wide operations and moving the cluster from one consistent state to another e.g. cache creation/destroy,
a node JOIN/LEFT/FAIL operations, snapshot creation, etc. During the switching process, all user transactions are parked for a small
period of time which in turn increased the average latency and throughput of the overall cluster.

Splitting the cluster into virtual cells containing 4-8 nodes may increase the total cluster performance and minimize the
influence of one cell on another in case of node fail events. Such a technique also significantly increase transactions recovery 
speed on cells not affected by failing nodes. The time which transactions are parked also decreasing on not affected cells which 
in turn decreases the worst latency for the cluster operations overall.

From now on you can use the `RendezvousAffinityFunction` affinity function with the `ClusterNodeAttributeColocatedBackupFilter` to
group nodes into virtual cells. Since the node baseline attributes are used as cell markers the corresponding 
[BASELINE_NODE_ATTRIBUTES](https://ignite.apache.org/docs/latest/monitoring-metrics/system-views#baseline_node_attributes) system
view was added.

See benchmarks below which represent the worst (max) latency that happens in case of node left/failure/timeout events on broken 
and alive cells.

![transactions_cell_switch](../_img/switch_and_recovery_cells.png)


## New Page Replacement Policies

When Native Persistence is on and the amount of data, which Ignite stores on the disk, is bigger than the off-heap memory amount 
allocated for the data region, another page should be evicted from the off-heap to the disk to preload a page from the disk to 
the completely full off-heap memory. This process is called page replacement. Previously, Apache Ignite used the Random-LRU page 
replacement algorithm which has a low maintenance cost, but it has many disadvantages and greatly affects the performance when 
the page replacement is started. On some deployments, administrators even force a cluster restart periodically to avoid page 
replacement. There are a few new algorithms available from now on:
- Segmented-LRU Algorithm
- CLOCK Algorithm

Page replacement algorithm can be configured by the `PageReplacementMode` property of `DataRegionConfiguration`. By default, now 
the CLOCK algorithm is used. You can check the 
[Replacement Policies](https://ignite.apache.org/docs/latest/memory-configuration/replacement-policies) at documentation pages 
for more details.


## Snapshot Restore And Check Commands
### Check

All snapshots are fully consistent in terms of concurrent cluster-wide operations as well as ongoing changes with Ignite.
However, in some cases and for your own peace of mind it may be necessary to check the snapshot for completeness and 
for data consistency. The Apache Ignite now is delivered with built-in snapshot consistency check commands that enable you to 
verify internal data consistency, calculate data partitions hashes and pages checksums, and print out the result if a problem 
is found. The check command also compares hashes calculated by containing keys of primary partitions with corresponding backup 
partitions and reports any differences.

```shell
# This procedure does not require the cluster to be in the idle state.
control.(sh|bat) --snapshot check snapshot_name
```

### Restore

Previously, only the manual snapshot restore procedure have been available by fully copying persistence data files from the 
snapshot directory to the Apache Ignite `work` directory. The automatic restore procedure allows you to restore cache groups from
a snapshot on an active cluster by using the Java API or command line script (using CLI is recommended).  Currently, the restore 
procedure has several limitations, so check the documentation pages for details.

```shell
Start restoring all user-created cache groups from the snapshot "snapshot_09062021".
control.(sh|bat) --snapshot restore snapshot_09062021 --start

# Start restoring only "cache-group1" and "cache-group2" from the snapshot "snapshot_09062021".
control.(sh|bat) --snapshot restore snapshot_09062021 --start cache-group1,cache-group2

# Get the status of the restore operation for "snapshot_09062021".
control.(sh|bat) --snapshot restore snapshot_09062021 --status

# Cancel the restore operation for "snapshot_09062021".
control.(sh|bat) --snapshot restore snapshot_09062021 --cancel
```
