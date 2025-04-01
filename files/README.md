Benchmark for the `files` module, which provides and overview on CASSANDRA-20333.

The order
- 20333-optimistic
- 20333-optimistic cluster warmed
- trunk
- trunk cluster warmed

### Configuration

cassandra.yaml:
```
memtable:
  configurations:
    skiplist:
      class_name: SkipListMemtable
    trie:
      class_name: TrieMemtable
    default:
      inherits: trie

memtable_allocation_type: offheap_objects
```

- Config: 100 keyspaces with 1 table each
- Reads: 10%
- Read Op: `SELECT * FROM system_metrics.keyspace_group WHERE name = 'o.a.c.m.k.MutationSizeHistogram.${KEYSPACE}${keyspace_index}';`
- Writes: 90%
- Write Op: `INSERT INTO ${KEYSPACE}.${TABLE} (id) VALUES (:id);`
- Run Cycles: 10000000
- Async Threads: 256

```
latte schema workloads/basic/write-read-mixed.rn                                         
latte run workloads/basic/write-read-mixed.rn -f read:0.1 -f write:0.9 -p 256 -d 10000000
```

Compaction is disabled to exaggerate metrics impact:
```
./nodetool disableautocompaction
```

JVM extra options:
```
-XX:+UnlockDiagnosticVMOptions -XX:+DebugNonSafepoints
```

