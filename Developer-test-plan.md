=== Node-Agent
* Make sure provisioner node allocation is fine and doesn’t keep rotating amongst the nodes
* The tasks targeted at provisioner (e.g. expand cluster) should be picked properly by provisioner node and processed
SDS detection should happen properly with the details and import cluster should be successful

=== Gluster Integration
* Import cluster should be successful and all the cluster details should be populated properly
* During import + un-manage multiple times it should work well and cluster should not end up in inconsistent/unusable state
* Volume level actions profiling should work as expected and state of the volumes and bricks should be updated properly in UI. No details should be missing and it should be matching with CLI output from gluster side

=== Expand Cluster
* When new nodes peer probed from CLI, an notification should appear in UI telling about running tendrl-ansible for additional nodes
* Once tendrl-ansible executed for additional node, the above notification alert should be cleared and cluster should be marked as `Expand Pending` in UI
* User should able to click the expand cluster from UI and job should succeed properly. The new node details should be visible in UI
* If expand cluster fails (say due to wrong repos configured for gluster on node), the same should be marked properly and un-manage should be possible for the cluster
* Once un-manage done and above repos error corrected on the node, the import cluster should be possible and successful as expected

=== Overall end-to-end testing
* Make sure import is successful and all the details of the cluster like volumes, bricks etc get reflected properly in UI
* All the grafana dashboards should populate the details properly
* All the volume level actions (e.g. enable/disable profiling) should work as expected
* All the cluster level actions (e.g. un-manage, expand, enable/disable profiling) should work as expected
