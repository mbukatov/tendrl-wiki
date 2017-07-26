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

Graphite table name: tendrl.clusters.<cluster-id>.nodes.<node-name>.cpu.<attr-type>

Note:
Source for above descriptions is : http://docs.rightscale.com/faq/How_do_the_CPU_Metrics_work_and_what_is_CPU_Steal.html

### Memory

* memory-buffered:Amount of memory used for buffering, mostly for I/O operations
* memory-free: Total amount of unused memory
* memory-used: Total amount of memory used
* memory-cached: Memory used for caching disk data for reads, memory-mapped files or tmpfs data
* percent-buffered: Percentage of of memory used for buffering, mostly for I/O operations
* percent-cached: Percentage of memory used for caching disk data for reads, memory-mapped files or tmpfs data
* percent-free: Percentage of unused memory
* memory-slab_recl: Amount of reclaimable memory used for slab kernel allocations
* memory-slab_unrecl: Amount of unreclaimable memory used for slab kernel allocations
* percent-slab_recl: Percentage of reclaimable memory used for slab kernel allocations
* percent-slab_unrecl: Percentage of unreclaimable memory used for slab kernel allocations
* percent-used: Percentage of memory used

Graphite table name: tendrl.clusters.<cluster-id>.nodes.<node-name>.memory.<attr-type>
Note:
Source of descriptions: https://github.com/signalfx/integrations/tree/master/collectd-memory/docs

### Mount-point(df)

* df_complex-free: This metric measures free disk space in bytes on this file system.
* df_complex-reserved: This metric measures disk space in bytes reserved for the super-user on this file system.
* df_complex-used: This metric measures used disk space in bytes on this file system.
* df_inodes-free: This metric measures free inodes in the file system. Inodes are structures used by Unix filesystems to store metadata about files.
* df_inodes-reserved: This metric measures inodes reserved for the super user in the file system. Inodes are structures used by Unix filesystems to store metadata about files.
* df_inodes-used: This metric measures used inodes in the file system. Inodes are structures used by Unix filesystems to store metadata about files.
* percent_bytes-free: This metric measures free disk space as a percentage of total disk space on this file system.
* percent_bytes-reserved: This metric measures disk space reserved for the super-user as a percentage of total disk space of this file system.
* percent_bytes-used: This metric measures used disk space as a percentage of total disk space of this file system.
* percent_inodes-free: This metric measures free inodes as a percentage of total inodes in the file system. Inodes are structures used by file systems to store information about files (other than its content).
* percent_inodes-reserved: This metric measures inodes reserved for the super-user as a percentage of total inodes in the file system. Inodes are structures used by file systems to store information about files (other than its content).
* percent_inodes-used: This metric measures used inodes as a percentage of total inodes in the file system. Inodes are structures used by file systems to store information about files (other than its content).

Graphite table name: tendrl.clusters.<cluster-id>.nodes.<node-name>.df-<mount-point>.<attr-type>
Note:
Source of descriptions: https://github.com/signalfx/integrations/blob/master/collectd-df/docs/

### disk
* disk_io_time
  ** io_time
  ** weighted_io_time
* disk_merged
  ** read : The number of disk reads that have been merged into single physical disk access operations. In other words, this metric measures the number of instances in which one physical disk access served multiple disk reads.
  ** write: The number of disk writes that were merged into single physical disk access operations. In other words, this metric measures the number of instances in which one physical disk access served multiple write operations.
* disk_octets
  ** read : The number of bytes read from a disk.
  ** write: The number of bytes written to a disk.
* disk_ops
  ** read : The number of disk read operations.
  ** write : The number of disk write operations.
* disk_time
  ** read : The average amount of time it took to do a read operation. For Darwin / Mac OS X, the unit is microseconds. For Linux and AIX, the unit is milliseconds. For Solaris, the unit is nanoseconds. This metric is not reported on FreeBSD.
  ** write : The average amount of time it took to do a write operation. For Darwin / Mac OS X, the unit is microseconds. For Linux and AIX, the unit is milliseconds. For Solaris, the unit is nanoseconds. This metric is not reported on FreeBSD.

Graphite table name: tendrl.clusters.<cluster-id>.nodes.<node-name>.disk-<disk-name>.<attr-type1>.<attr-type2>
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

Graphite table name: tendrl.clusters.<cluster-id>.nodes.<node-name>.swap.<attr-type>

### ping
* ping-<monitoring-integration fqdn> : Ping latency(round-trip time) from current node to monitoring-integration node
* ping_droprate-<monitoring-integration fqdn>: Ping packet drop rate from current node to monitoring-integration node
* ping_stddev-<monitoring-integration fqdn>: Standard deviation of Ping latency.

Graphite table name: tendrl.clusters.<cluster-id>.nodes.<node-name>.ping.<attr-type>

### network_throughput-cluster_network

This is calculated as summation of rx(incoming/received packets) an tx(outgoing/transmitted packets) corresponding to the network interface used/marked during cluster creation.

Graphite table name: tendrl.clusters.<cluster-id>.nodes.<node-name>.network_throughput-cluster_network.gauge-used

Note: This rx and tx are maintained as continuous counters and hence the plugin takes interval diff for a period of 1 second.

