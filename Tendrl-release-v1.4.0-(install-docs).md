- **Latest packages**: https://copr.fedorainfracloud.org/coprs/tendrl/release/packages/
- **Latest dependencies**: https://copr.fedorainfracloud.org/coprs/tendrl/dependencies/packages/
- **Install**: https://github.com/Tendrl/documentation/wiki/Tendrl-Package-Installation-Reference


**Pre-requisites (mandatory):**
- Install tendrl-node-agent on the node running tendrl-api and central store
- tendrl-release v1.4.0 is not backward compatible with central store for tendrl-release v1.3.0, please use a new central store (etcd) instance, old data will not be migrated.
- The Node hosting tendrl-api/central_store should have minimum 12 GB of memory and 4 VCPUs (or equivalent)

**Pre-requisites for Creating Ceph cluster via tendrl-api**
- Install ceph-installer <=1.3.0 on the tendrl-api node
- Configure appropriate ceph repositories on all cluster nodes as required by ceph-installer
- Disable gpg key check for ceph repositories on all cluster nodes

**Pre-requisites for Creating and Importing Gluster cluster via tendrl-api**
- On any **one** node in the gluster cluster, Append the tag "provisioner/gluster" to "tags" list in /etc/tendrl/node-agent/node-agent.conf.yaml 
- This "provisioner/gluster" node will be selected to install Gdeploy and python-gdeploy by Tendrl

**Additional pre-requisites for Importing existing Gluster cluster via tendrl-api**
- All nodes in existing Gluster cluster should be peer probed (i.e. present in gluster trusted storage pool) already (for ImportCluster), only after which tendrl-node-agent should be installed on all nodes, without peer probe, the node wont be detected by tendrl as a gluster node.