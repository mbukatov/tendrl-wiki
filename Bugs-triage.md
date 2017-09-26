## 21 Sep 2017

**Tendrl/api**

* [Configuration file /etc/tendrl/etcd.yml with etcd root password is readable for every account](https://github.com/Tendrl/api/issues/293)
* [postinstall and postuninstall scripts should not contain `restorecon -Rv /`](https://github.com/Tendrl/api/issues/291)
* ~~[Volumes API is returning incomplete response](https://github.com/Tendrl/api/issues/289)~~

**Tendrl/ui**
* [task progress - cancel button](https://github.com/Tendrl/ui/issues/589)
* [Tendrl - Blank Slate (for Clusters List) showing wrong / old message](https://github.com/Tendrl/ui/issues/590)   -- **Fixed, Need Verification** 
* [Grafana dashboards fail to launch from Tendrl UI](https://github.com/Tendrl/ui/issues/623)
* [Admin area should be launched from the utilities in the top right (and not the left navigation bar)](https://github.com/Tendrl/ui/issues/628)
* [There is "Launch Dashboard" link for hosts in Hosts page when cluster is not yet imported](https://github.com/Tendrl/ui/issues/610)  -- **Fixed, Need Verification**
* [Cluster ID in Host List does not match the Cluster ID in the Cluster List or context switcher](https://github.com/Tendrl/ui/issues/622)  -- **Fixed, Need Verification**
* [Inconsistent behavior in how we see object details](https://github.com/Tendrl/ui/issues/627)
* [UI Issues with Notification Drawer](https://github.com/Tendrl/ui/issues/629)  -- **Fixed, Need Verification**
* [Task Details: Scroll Events so most recent are always in view](https://github.com/Tendrl/ui/issues/630)  -- **Fixed, Need Verification**
* [Align Save and C**Tendrl/ui**ancel buttons with left edge of input fields on all forms](https://github.com/Tendrl/ui/issues/631)  -- **Fixed, Need Verification**

**Tendrl/commons**
* None

**Tendrl/node-agent**
* [systemd service file should contain Documentation section](https://github.com/Tendrl/node-agent/issues/613)
* [Configuration file node-agent.conf.yaml with etcd root password is readable for every account](https://github.com/Tendrl/node-agent/issues/625)
* [Sync object grouping and intervals](https://github.com/Tendrl/node-agent/issues/626)
* [When rpm package installation fails during Import Cluster operation, information about this problem is not provided](https://github.com/Tendrl/node-agent/issues/627)
* ~~[Brick Disk stats collectd plugin sums all partitions for a partitioned disk stats](https://github.com/Tendrl/node-agent/issues/628)~~

**Tendrl/gluster-integration**
* [Enable/Disable profiling doesn't work consistently](https://github.com/Tendrl/gluster-integration/issues/405)   -- **Fixed, Need Verification**
* [No alerts/notifications generated when volume and brick are almost full (and possible data discrepancy](https://github.com/Tendrl/gluster-integration/issues/412)
* [Volume profiling flag is not set as instructed always](https://github.com/Tendrl/gluster-integration/issues/418)  -- **Fixed, Need Verification**
* [ postinstall and postuninstall scripts should not contain `restorecon -Rv /`](https://github.com/Tendrl/gluster-integration/issues/424)

**Tendrl/monitoring-integration**
* [systemd service file should contain Documentation section](https://github.com/Tendrl/monitoring-integration/issues/77)
* [monitoring-integration_logging.yaml not touched after service reload](https://github.com/Tendrl/monitoring-integration/issues/78)
* [Grafana Dashboard Labels misleading](https://github.com/Tendrl/monitoring-integration/issues/79)
* [Gluster - Host At-A-Glance (Grafana host dashboard): Label and y-axis metric changes](https://github.com/Tendrl/monitoring-integration/issues/91)  -- **Fixed, Need Verification**
* [Historical data for graphs in Grafana older than 24 hours are missing](https://github.com/Tendrl/monitoring-integration/issues/95)  -- **Fixed, Need Verification**
* [Healing data is not displayed on the dashboard](https://github.com/Tendrl/monitoring-integration/issues/120)  -- **Fixed, Need Verification**
* [Configuration file monitoring-integration.conf.yaml with etcd root password is readable for every account](https://github.com/Tendrl/monitoring-integration/issues/125)
* ~~[Decimal correction for the counter panels](https://github.com/Tendrl/monitoring-integration/issues/130)~~

**Tendrl/notifier**
* ~~[Default configuration of smtp server contains real domain names with MX entries and one unregistered (so far)](https://github.com/Tendrl/notifier/issues/115)~~

**Tendrl/tendrl-ansible**

## 26 Sep 2017

**Tendrl/api**
* [Internal Server Error when user does not exist ](https://github.com/Tendrl/api/issues/300)
* [Required: Internal profiling for tendrl-api](https://github.com/Tendrl/api/issues/301)
* [Sample SSL configuration redirects to ip address](https://github.com/Tendrl/api/issues/302)

**Tendrl/ui**
* [Spell Checker on the UI repo](https://github.com/Tendrl/ui/issues/636)
* [Fix responsiveness fro brick details page](https://github.com/Tendrl/ui/issues/637)
* [Filter controls don’t seem to match PatternFly Filter control](https://github.com/Tendrl/ui/issues/638)

**Tendrl/gluster-integration**
* [Performance testing gluster-integration to optimise sync cycles](https://github.com/Tendrl/gluster-integration/issues/429)
* [Update minimum required gluster version to 3.12.2](https://github.com/Tendrl/gluster-integration/issues/432)
* [On deleting a resource there is a need to update graphite data](https://github.com/Tendrl/gluster-integration/issues/435)

**Tendrl/monitoring-integration**
* [Grafana Dashboards are not using SSL when SSL is configured for Tendrl](https://github.com/Tendrl/monitoring-integration/issues/144)
* [Need to add this repository in coverall and codacy 3rd party tools to calculate code quality](https://github.com/Tendrl/monitoring-integration/issues/143)
* [Add code quality check plugins for pull request](https://github.com/Tendrl/monitoring-integration/issues/141)
