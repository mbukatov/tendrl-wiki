**Metrics availability based on milestones:** https://docs.google.com/document/d/1oX10aH-jMqTqkZkip4eNAevwT8GgcG6_XxAgQdVnmNI/edit

# List of metrics and their respective attributes currently made available via collectd plugins:

## Node Level

### CPU

* percent-idle - CPU not doing any work
* percent-interrupt - CPU allocated to hardware interrupts
* percent-nice - CPU used to allocate multiple processes demanding more cycles than the CPU can provide
* percent-softirq - CPU servicing soft interrupts
* percent-steal - Steal time is the time that the CPU had something runnable, but the XEN hypervisor chose to run something else instead.
* percent-system - CPU used by the operating system itself
* percent-user - CPU used by user applications
* percent-wait - CPU waiting for disk IO operations to complete

Graphite table name: tendrl.clusters.{cluster-id}.nodes.{node-name}.cpu.{attr-type}

Note:
Source for above descriptions is: http://docs.rightscale.com/faq/How_do_the_CPU_Metrics_work_and_what_is_CPU_Steal.html

### Memory

* memory-buffered:Amount of memory used for buffering, mostly for I/O operations
* memory-free: Total amount of unused memory
* memory-used: Total amount of memory used
* memory-cached: Memory used for caching disk data for reads, memory-mapped files or tmpfs data
* percent-buffered: Percentage of memory used for buffering, mostly for I/O operations
* percent-cached: Percentage of memory used for caching disk data for reads, memory-mapped files or tmpfs data
* percent-free: Percentage of unused memory
* memory-slab_recl: Amount of reclaimable memory used for slab kernel allocations
* memory-slab_unrecl: Amount of unreclaimable memory used for slab kernel allocations
* percent-slab_recl: Percentage of reclaimable memory used for slab kernel allocations
* percent-slab_unrecl: Percentage of unreclaimable memory used for slab kernel allocations
* percent-used: Percentage of memory used

Graphite table name: tendrl.clusters.{cluster-id}.nodes.{node-name}.memory.{attr-type}
Note:
Source of descriptions: https://github.com/signalfx/integrations/tree/master/collectd-memory/docs

### Mount-point(df)

* df_complex-free: This metric measures free disk space in bytes on this file system.
* df_complex-reserved: This metric measures disk space in bytes reserved for the super-user on this file system.
* df_complex-used: This metric measures used disk space in bytes on this file system.
* df_inodes-free: This metric measures free inodes in the file system. Inodes are structures used by Unix file systems to store metadata about files.
* df_inodes-reserved: This metric measures inodes reserved for the super user in the file system. Inodes are structures used by Unix file systems to store metadata about files.
* df_inodes-used: This metric measures used inodes in the file system. Inodes are structures used by Unix file systems to store metadata about files.
* percent_bytes-free: This metric measures free disk space as a percentage of total disk space on this file system.
* percent_bytes-reserved: This metric measures disk space reserved for the super-user as a percentage of total disk space of this file system.
* percent_bytes-used: This metric measures used disk space as a percentage of total disk space of this file system.
* percent_inodes-free: This metric measures free inodes as a percentage of total inodes in the file system. Inodes are structures used by file systems to store information about files (other than its content).
* percent_inodes-reserved: This metric measures inodes reserved for the super-user as a percentage of total inodes in the file system. Inodes are structures used by file systems to store information about files (other than its content).
* percent_inodes-used: This metric measures used inodes as a percentage of total inodes in the file system. Inodes are structures used by file systems to store information about files (other than its content).

Graphite table name: tendrl.clusters.{cluster-id}.nodes.{node-name}.df-{mount-point}.{attr-type}
Note:
Source of descriptions: https://github.com/signalfx/integrations/blob/master/collectd-df/docs/

### disk
* disk_io_time
  * io_time: The disk I/O time in milliseconds (ms).
  * weighted_io_time: The aggregate time in milliseconds (ms) spent on I/O operations that are either in progress or have completed.
* disk_merged
  * read: The number of disk reads that have been merged into single physical disk access operations. In other words, this metric measures the number of instances in which one physical disk access served multiple disk reads.
  * write: The number of disk writes that were merged into single physical disk access operations. In other words, this metric measures the number of instances in which one physical disk access served multiple write operations.
* disk_octets
  * read: The number of bytes read from a disk.
  * write: The number of bytes written to a disk.
* disk_ops
  * read: The number of disk read operations.
  * write: The number of disk write operations.
* disk_time
  * read: The average amount of time it took to do a read operation. For Darwin / Mac OS X, the unit is microseconds. For Linux and AIX, the unit is milliseconds. For Solaris, the unit is nanoseconds. This metric is not reported on FreeBSD.
  * write: The average amount of time it took to do a write operation. For Darwin / Mac OS X, the unit is microseconds. For Linux and AIX, the unit is milliseconds. For Solaris, the unit is nanoseconds. This metric is not reported on FreeBSD.

Graphite table name: tendrl.clusters.{cluster-id}.nodes.{node-name}.disk-{disk-name}.{attr-type1}.{attr-type2}
Note:
Source of descriptions: https://github.com/signalfx/integrations/blob/master/collectd-disk/docs/

### swap

* percent-cached: 
* percent-free
* percent-used
* swap-cached
* swap-free
* swap_io-in
* swap_io-out
* swap-used

Graphite table name: tendrl.clusters.{cluster-id}.nodes.{node-name}.swap.{attr-type}

### ping

* ping-{monitoring-integration fqdn} : Ping latency(round-trip time) from current node to monitoring-integration node
* ping_droprate-{monitoring-integration fqdn}: Ping packet drop rate from current node to monitoring-integration node
* ping_stddev-{monitoring-integration fqdn}: Standard deviation of Ping latency.

Graphite table name: tendrl.clusters.{cluster-id}.nodes.{node-name}.ping.{attr-type}

### network_throughput-cluster_network

This is calculated as a summation of rx(incoming/received packets) a tx(outgoing/transmitted packets) corresponding to the network interface used/marked during cluster creation.

Graphite table name: tendrl.clusters.{cluster-id}.nodes.{node-name}.network_throughput-cluster_network.gauge-used

Note: This rx and tx are maintained as continuous counters and hence the plugin takes interval diff for a period of 1 second.


## Gluster Cluster

### inode_utilization

* gauge-total
* gauge-used
* percent-percent_bytes

Inode utilization at the level of bricks fetched using os.statvfs on brick path
Graphite table name:

* tendrl.clusters.{cluster-id}.volumes.{volume-name}.nodes.{node-name}.bricks.{brick-path}.inode_utilization.{attr-
type}
* tendrl.nodes.{node-name}.bricks.{brick-path}.inode_utilization.{attr-type}

### iops

* gauge-read
* gauge-write

Brick level read and write operations fetched using gluster volume profile info
Graphite table name:
* tendrl.clusters.{cluster-id}.volumes.{volume-name}.nodes.{node-name}.bricks.{brick-path}.iops.{attr-type}

### utilization

* gauge-total
* gauge-used
* percent-percent_bytes

Brick utilization fetched using os.statvfs on brick path
Graphite table name: 
* tendrl.clusters.{cluster-id}.volumes.{volume-name}.nodes.{node-name}.bricks.{brick-path}.utilization.{attr-type}
* tendrl.nodes.{node-name}.bricks.{brick-path}.utilization.{attr-type}

### No. Of connections(clients)

No. of connections to the gluster volume fetched using: gluster volume status all clients --xml
Note: This also includes brick connections in the counter.
Graphite table name:
* tendrl.clusters.{cluster-id}.volumes.{volume-name}.connections_count

### Volume Status

The source for this information is gluster get-state glusterd odir /var/run file ...
The volume status is encoded as follows:
* 'Started': 0
* 'Degraded': 1
* 'Stopped': 2
Note: Count of degraded volumes is currently not made available yet.

Graphite table name:
* tendrl.clusters.{cluster_id}.volumes.{volume_name}.status

### Node(Peer) Status wise counter at cluster level

The source for this information is gluster get-state glusterd odir /var/run file ...
The following counters are made available:
* Down: Nodes that are not marked ['Peer', 'in', 'Cluster'] or peer['connected'] == 'Connected'
* Total

Graphite table name:
* tendrl.clusters.{cluster_id}.nodes_count.down
* tendrl.clusters.{cluster_id}.nodes_count.total

### Volume status wise counts

The raw source for this information is gluster get-state glusterd odir /var/run file ...
The following status-wise counters are made avaialable:

* total
* down

Note: Degraded volume counter needs to be added

Graphite table name:
* tendrl.clusters.{cluster-id}.volume_count.total
* tendrl.clusters.{cluster-id}.volume_count.down

* Brick count

The raw source for this information is gluster get-state glusterd odir /var/run file ...

Graphite table name:
* tendrl.clusters.<cluster_id>.brick_count.total

### Volume level

#### Bricks count

The source for this information is gluster get-state glusterd odir /var/run file ...

Graphite table name:
* tendrl.clusters.{cluster-id}.volumes.{volume-name}.bricks_count

#### Status

The source for this information is gluster get-state glusterd odir /var/run file ...

Graphite table name:
* clusters.{cluster_id}.volumes.{volume_name}.status

##  Screenshots
### Gluster at a Glance
![](https://github.com/cloudbehl/grafana-dashboards/blob/master/snapshots/Tendrl-At-A-Glance.png)

### Gluster Hosts
![](https://github.com/cloudbehl/grafana-dashboards/blob/master/snapshots/Tendrl-Hosts.png)

### Gluster Bricks
![](https://github.com/cloudbehl/grafana-dashboards/blob/master/snapshots/Tendrl-Bricks.png)

### Gluster Volumes
![](https://github.com/cloudbehl/grafana-dashboards/blob/master/snapshots/Tendrl-Volume.png)
![](https://github.com/cloudbehl/grafana-dashboards/blob/master/snapshots/Tendrl-Volume1.png)