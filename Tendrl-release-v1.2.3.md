This release includes

* Fixes for the bugs/defects/topics reported on previous builds
* APIs for Create {Ceph Cluster, Pool, RBDs, EC Profile}
* APIs for Update/Delete {Pool, RBDs, EC Profile)
* APIs for Create {Gluster Cluster, Brick, Volume}
* APIs for Update/Delete {Volume)
* Performance and node monitoring 
* Alerting (node and cluster)

Pre-requisites for Creating Ceph cluster via tendrl-api
- Install tendrl-node-agent on node which is running tendrl-api and central store
- Install **[ceph-installer <=1.2.2](https://www.redhat.com/archives/tendrl-devel/2017-April/msg00036.html)** on the tendrl-api node
- Configure appropriate ceph repositories on all cluster nodes as required by ceph-installer
- Disable gpg key check for ceph repositories on all cluster nodes