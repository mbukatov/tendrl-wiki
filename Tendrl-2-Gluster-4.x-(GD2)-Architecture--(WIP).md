# Tendrl 2 + Gluster 4.x (GD2) Architecture

Arch Diagram:
![Link](https://i.imgur.com/pQqiiAZ.jpg)
**Requirements**: 
- At Parity with monitoring/alerting/notification capabilities and metrics provided by Tendrl 1.x
https://github.com/Tendrl/documentation/wiki/metrics
- Agentless, server-only, orchestration and control model with reduced overall resource consumption
- Ability to execute/orchestrate multi node ansible (eg: gluster-ansible)  based jobs (eg: ImportCluster, ExpandCluster, CreateCluster, CreateVolume etc) via GD2 Rest API
- Integration via GD2 rest API and deprecation of RHGS “get-state” cli calls for fetching cluster topology.
- Metrics collection via Prometheus, aggregation via Tendrl2 service, Alerting and Notifications via Prometheus and  deprecation of graphite/collectd stack (unless we want to support it)
- Fully containerized w.r.t to Kubernetes/OpenShift/CNS use cases
- Install strategy for Tendrl2 (Standalone), Tendrl2+Kubernetes/OpenShift.
- User management? (ldap, simple )
- Troubleshooting and log management through all the layers (Tendrl api/service, ansible, gluster processes) via common TendrlContext
- Fault tolerance for Tendrl service and other dependencies

**Details:**
**Example flows** (needs UX guidance)
- Import Cluster
   - User installs tendrl2-ansible package
   - In tendrl2-ansible, user provides hostname/ip for installing Tendrl server
   - User provides GD2 rest API endpoint/credentials via Tendrl UI/API
   - Tendrl discovers cluster topology using GD2 rest API, User can import the cluster via Tendrl UI/API
   - During Import, Tendrl will setup prometheus exporters on every node for node local metrics
   - Tendrl will setup one prometheus exporter on Tendrl Server per cluster for cluster metrics
   - Tendrl server will verify whether cluster is  successfully imported via multiple sources (GD2 Rest API, Tendrl cluster topology, Prometheus cluster status)

- Create Cluster
   - User installs tendrl2-ansible package
   - In tendrl2-ansible, user provides hostname/ip for installing Tendrl server and runs tendrl2-ansible
   - User provides list storage nodes (ip/fqdn) via Tendrl UI/API
   - Tendrl prepares the storage nodes, installs/configures GD2 and its rest API and calls ImportCluster with the GD2 rest API endpoint


**Assumptions:**
- Server or colocation with storage nodes for tendrl2 stack
- SSH access to all gluster nodes (requirement to run ansible, install node-local exporters)
- A single GD2 API endpoint can provide information and control the entire cluster
- Agentless, server-only, orchestration and control model with reduced overall resource consumption
- Deprecate all the Tendrl 1 node local services
- Metrics collection will be handled by Tendrl2 server via prometheus exporters/jobs
- Management operations will be handled centrally by Tendrl2 api/service via ansible
- Tendrl2 API/Service features:
   - Ability to execute/orchestrate/audit multi node ansible (eg: gluster-ansible) based jobs (eg: ImportCluster, ExpandCluster, CreateCluster, CreateVolume etc).
   - Remote metrics collection/aggregation from prometheus exporters/jobs for node as well as cluster metrics.
   - Fetch cluster topology from GD2 rest API.
 

**How it works:**
- export all data points to prometheus DB
- tendrl service pulls prometheus API, GD2 rest API for creating cluster topology objects in etcd.
- tendrl service aggregates/analyzes prometheus data and pushes it back as advanced metrics
- tendrl service runs jobs against etcd schema as is currently the case
- tendrl service jobs gain added functionality to be able to run job functionality on any node through ansible

**Scaling and other reading:**
- https://prometheus.io/blog/2016/07/23/pull-does-not-scale-or-does-it/
- https://prometheus.io/blog/2016/07/23/pull-does-not-scale-or-does-it/#it-doesn-t-matter-who-initiates-the-connection
- https://prometheus.io/docs/introduction/comparison/
- To put some numbers behind this: a single big Prometheus server can easily store millions of time series, with a record of 800,000 incoming samples per second (as measured with real production metrics data at SoundCloud). Given a 10-seconds scrape interval and 700 time series per host, this allows you to monitor over 10,000 machines from a single Prometheus server.

