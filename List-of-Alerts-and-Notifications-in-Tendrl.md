Tendrl generates alerts primarily from following channels:
* grafana
* gluster integration state sync
* glusterfs native events

These alerts can be broadly classified into following 3 categories:
* **Status alerts** : These are alerts that are due to change in state/status of some entity that is being managed by tendrl.
* **Utilization alerts**: These are alerts that are due to utilization of some entity crossing the threshold. Both breaching the threshold and comming back to normal state.
* **gluster native events(notify only)**: There are some events that are notified by gluster native eventing API for which tendrl cannot maintain an alert state as these entities are not managed by tendrl(eg. File with `gfid` is corrupted ). However its would help a gluster admin if he gets notified for these events. So for these events tendrl will only notify through all the notification channels configured. 

NOTE: status and utilization alerts have clearing alerts i.e if the alert condition is resolved a clearing alert will be generated that will replace the original alert in UI and also notifications will be sent in all available channels. gluster native events do not have clearing alerts as it is not provided by the glusterfs native events API.

You can find the list of tendrl alerts below, along with the entities those alerts will be affecting (alert count for the mentioned entity will be incremented on receiving  this alert)

## Status alerts

### Sds related alerts
* volume status (volume, cluster)
* volume state (volume, cluster)
* brick status (volume, host, cluster)
* peer status (cluster)
* rebalance status (volume, cluster)
* georeplication status (cluster)
* quorum of volume lost (volume, cluster)
* quorum of volume regained (volume, cluster)
* svc  connected (cluster)
* svc disconnected (cluster)
* minimum number of bricks not up in EC subvolume (volume, cluster)
* minimum number of bricks up in  EC subvolume (volume, cluster)
* afr quorum met for subvolume (volume, cluster)
* afr quorum fail for subvolume (volume, cluster)
* afr subvolume up (volume, cluster)
* afr subvolume down (volume, cluster)

## Utilization alerts

### Host related alerts
* cpu utilization (host)
* memory utilization (host)
* swap utilization (host)

### Sds related alerts
* volume utilization (volume, cluster)
* brick utilization (volume, cluster)

## Gluster native events

* Peer has moved to unknown state
* Brick path resolution failed for brick
* Quota soft limit crossed in volume for volume
* File with gfid corrupted due to bitrot  in brick
* Subvolume affected by split-brain
* Snapshot soft limit reached for volume
* Snapshot hard limit reached for volume
* Compare friend volume failed for volume
* Posix health check failed for brick
* Peer rejected
* Rebalance status update failed for volume
* Scv reconfigure failed for service
* Georeplication checkpoint completed for session