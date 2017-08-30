The following are the specific links to [Tendrl UI Designs](https://github.com/Tendrl/documentation/wiki/Tendrl-UI-designs) related to Gluster-specific monitoring using Tendrl (excludes Day 1 & 2 operations and Ceph management/monitoring):

[First Login by admin User](https://redhat.invisionapp.com/share/6T900V2ZX) - Password reset for first-time login by the default admin user after Tendrl installed using [tendrl-ansible](https://github.com/Tendrl/tendrl-ansible).  If no clusters found, user sees a [blank slate](https://redhat.invisionapp.com/share/6T900V2ZX#/screens/198042644). 

Upon successful login and if a cluster is detected (after tendrl-ansible completed, user will go through the following [workflow, and see the cluster list as the landing page] (https://redhat.invisionapp.com/share/8QCOEVEY9).
* Related GitHub Issue: https://github.com/Tendrl/specifications/issues/200

If user clicks the volume name (hyperlink) from the [Volumes List](https://redhat.invisionapp.com/share/8QCOEVEY9#/screens/244738623), they should see the following:
object details for a volume — initially just the [Bricks tab](https://redhat.invisionapp.com/share/XMAOW3UC5#/screens/221662357)
* For distributed, replicated volumes, bricks should be grouped by replica set
* For distributed, replicated volumes, bricks should be grouped by dispersed set

If user clicks the host name (hyperlink) from the [Hosts List](https://redhat.invisionapp.com/share/8QCOEVEY9#/screens/244738620), they should see the following:
object details for a host — initially just the Bricks tab
* It’ll be similar to the [Volumes > Bricks page](https://redhat.invisionapp.com/share/XMAOW3UC5#/screens/221662357) without the subvolume (replica / dispersed set) grouping.

Administration
* Events and Tasks are cluster-specific menu items that will be in the left-navigation menu --
 see https://redhat.invisionapp.com/share/8QCOEVEY9#/screens/244738616
* Users, Mail Settings, SNMP Settings, and Alerts / Notifications will be be accessed from the top right (Common Utilities/Elements area)
    * [Users, Mail Settings, SNMP Settings Menu](https://redhat.invisionapp.com/share/8QCOEVEY9#/screens/244738612)
    * [User management](https://redhat.invisionapp.com/share/8QCOEVEY9#/screens/244738633) -- Additional user management design details can be found at https://redhat.invisionapp.com/share/KNB25OEQT#/screens/226063802.
    * [SMTP/Mail configuration](https://redhat.invisionapp.com/share/8QCOEVEY9#/screens/244738631)
    * [SNMP configuration](https://redhat.invisionapp.com/share/8QCOEVEY9#/screens/244738632)
    * [Alerts / Notification Drawer](https://redhat.invisionapp.com/share/8QCOEVEY9#/screens/244738613)

Dashboard (for Grafana) designs - that will be launchable from the Tendrl UI:
* [Cluster](https://github.com/Tendrl/specifications/issues/222)
* [Host](https://github.com/Tendrl/specifications/issues/223)
* [Volume](https://github.com/Tendrl/specifications/issues/224)
* [Brick](https://github.com/Tendrl/specifications/issues/230)


To see more Tendrl UI designs, refer to the [Tendrl UI Designs Wiki](https://github.com/Tendrl/documentation/wiki/Tendrl-UI-designs).

