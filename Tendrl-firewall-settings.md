
## Ports to be enabled on server
* ETCD : 2379/tcp
* Graphite : 2003/tcp
* GraphiteWeb : 10080/tcp
* Tendrl Server : 80/tcp(http) or 443/tcp(https)
* Tendrl Api : 9292/tcp
* Grafana Server : 3000/tcp

**Note**: considering etcd, graphite, grafana and tendrl servers run on server machine.

## Ports to be enabled on storage node
There is no specific firewall setting to be done on node w.r.t tendrl. However enable all the ports needed for glusterfs as per gluster documentation.