Pre-requisites for Creating Ceph cluster via tendrl-api
- Install tendrl-node-agent on node which is running tendrl-api and central store
- For the above tendrl-node-agent, append below to "tags" list in /etc/tendrl/node-agent/node-agent.conf.yaml
  tags:
   - "provisioner/ceph"
- Install ceph-installer on the tendrl-api node
- Configure appropriate ceph repositories on all cluster nodes as required by ceph-installer
- Disable gpg key check for ceph repositories on all cluster nodes
- Disable selinux on all cluster nodes (fix in progress)