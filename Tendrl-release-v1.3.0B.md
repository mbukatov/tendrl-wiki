This beta2 release includes

* Fixes for the bugs/defects/topics reported on previous builds
* UI flows for Create {Ceph Cluster, Pool, RBDs}
* UI flows for Update/Delete {Pool, RBDs}
* UI flows for Create {Gluster Cluster, Brick, Volume}
* Improvements to Performance and node monitoring 
* Improvements to Alerting (node and cluster)
* Dashboard detail for Day2 administrative operations


- Latest packages: https://copr.fedorainfracloud.org/coprs/tendrl/release/packages/
- Latest dependencies: https://copr.fedorainfracloud.org/coprs/tendrl/dependencies/packages/
- Install: https://github.com/Tendrl/documentation/wiki/Tendrl-Package-Installation-Reference


Pre-requisites (mandatory):
- Install tendrl-node-agent on the node running tendrl-api and central store
- tendrl-release v1.3.0 is not backward compatible with central store for tendrl-release v1.2.3, please use a new central store (etcd) instance, old data will not be migrated.

Pre-requisites for Creating Ceph cluster via tendrl-api
- Install **[ceph-installer <=1.2.2](https://www.redhat.com/archives/tendrl-devel/2017-April/msg00036.html)** on the tendrl-api node
- Configure appropriate ceph repositories on all cluster nodes as required by ceph-installer
- Disable gpg key check for ceph repositories on all cluster nodes

Pre-requisites for Creating and Importing Gluster cluster via tendrl-api
- On any **one** node in the gluster cluster, Append the tag "provisioner/gluster" to "tags" list in /etc/tendrl/node-agent/node-agent.conf.yaml 
- This "provisioner/gluster" node will be selected to install Gdeploy and python-gdeploy by Tendrl

Additional pre-requisites for Importing existing Gluster cluster via tendrl-api
- All nodes in existing Gluster cluster should be peer probed already (for ImportCluster), only after which tendrl-node-agent should be installed on all nodes, without peer probe, the node wont be detected by tendrl as a gluster node.