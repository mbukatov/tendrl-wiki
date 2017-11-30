
## Ports to be enabled on server
* Etcd : 2379/tcp
* Graphite : 2003/tcp
* GraphiteWeb : 10080/tcp
* tendrl http : 80/tcp(http) or 443/tcp(https)
* tendrl-api : 9292/tcp
* Grafana Server : 3000/tcp
* tendrl-monitoring-integration : 8789/tcp  (grafana alerts callback)

**Note**: considering etcd, graphite, grafana and tendrl servers run on server machine.

## Ports to be enabled on storage node
Apart from all the ports needed for glusterfs as per gluster documentation.
* tendrl-gluster-integration: 8697/tcp (gluster native events webhook)