# Guidelines for filing bugs against Tendrl

Please follow these guidelines to file bugs with sufficient information to allow the Tendrl team to debug the issues. Before filing the bugs, please ensure that you've understood the roles and responsibilities of the various https://github.com/Tendrl/documentation/blob/master/tendrl-core-components-overview.adoc[components of the Tendrl core stack].

## Rendering related bugs

* There's a problem with the layout
* There's a problem while interacting with any of the UI elements
* UI elements don't behave as expected

File at: https://github.com/Tendrl/dashboard/issues

The following are NOT dashboard bugs:

* Wrong utilisation data displayed for ceph pools, file at: https://github.com/Tendrl/ceph_integration/issues
* Wrong information displayed during cluster import, follow the steps in the section below.

## Wrong data displayed

### State information

If the cluster state information displayed is wrong, it is more likely to be an API or a backend problem rather than a dashboard problem. The information in this category consists of the following:

* Cluster status (up/down/error etc.)
* List of nodes in the cluster and their roles
* Status (up/down/error etc.) of specific objects in the cluster (pools, osds, volumes etc.)
* Status of nodes (up/down/error etc.)

For any of these issues, use the following steps to file the bugs:

. Validate the output of `GetNodeList` and `GetClusterList` API calls.
.. Do the API calls provide the correct information? If yes, there's a problem with the dashboard displaying the information. File the bug at https://github.com/Tendrl/dashboard/issues. Include the output of both the API calls in the issue.
.. Do the API calls provide incorrect information?


