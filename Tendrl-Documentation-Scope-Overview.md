This is **work in progress**, I just want to sum up the current state before we can decide on next move. I will send an announcement on tednrl-devel when this page is ready for wider discussion.

This purpose of this wiki page is to clarify:

* scope of each type of documentation we have
* responsibilities among all tendrl contributors wrt the docs
* which docs are no longer up to date and needs to be dropped/updated
* which docs needs to be relocated

## Current state

* Documentation repository: https://github.com/Tendrl/documentation
  * [README](https://github.com/Tendrl/documentation/blob/master/README.adoc) - scope clarification needed
  * [Provisioning Ceph Cluster On CentOs](https://github.com/Tendrl/documentation/blob/master/create_ceph_cluster_on_centos.adoc) - dev notes about setting up Ceph cluster (no end user impact)
  * [Deploying Tendrl](https://github.com/Tendrl/documentation/blob/master/deployment.adoc) - description of installation (no longer up to date), also overview of architecture (no longer up to date)
  * [Tendrl Project Development Workflow](https://github.com/Tendrl/documentation/blob/master/development-workflow.adoc) - description of development, no longer up to date
  * [Glossary](https://github.com/Tendrl/documentation/blob/master/glossary.adoc) - looks good (minor improvements would help)
  * [Provisioning with Tendrl](https://github.com/Tendrl/documentation/blob/master/provisioning.adoc) - details about the provisioning internal desing (not sure if up to date)
  * [Tendrl Architectural Guidelines](https://github.com/Tendrl/documentation/blob/master/tendrl-architectural-guidelines.adoc) - dev guidelines (revisiting needed)
  * [Tendrl Core Components Overview](https://github.com/Tendrl/documentation/blob/master/tendrl-core-components-overview.adoc) - description of components, useful for end user, needs update
* Wiki: https://github.com/Tendrl/documentation/wiki
  * [list of tendrl talsk](https://github.com/Tendrl/documentation/wiki#talks-about-tendrl)
  * [How to file bugs against the Tendrl stack](https://github.com/Tendrl/documentation/wiki/How-to-file-bugs-against-the-Tendrl-stack) - dev and qe details, useful to end users who like to report a bug
  * [How to send alerts from inside Tendrl for devs](https://github.com/Tendrl/documentation/wiki/How-to-send-alerts-from-inside-Tendrl---for-devs) - internal dev details
  * [Information required for debugging issues on the Tendrl stack](https://github.com/Tendrl/documentation/wiki/Information-required-for-debugging-issues-on-the-Tendrl-stack) - qe details, useful for end users reporting bugs
  * [Labels on repositories](https://github.com/Tendrl/documentation/wiki/Labels-on-repositories) - dev details
  * [Tendrl Governance and Community](https://github.com/Tendrl/documentation/wiki/Tendrl-Governance-and-Community)
  * [Tendrl Package Installation Reference](https://github.com/Tendrl/documentation/wiki/Tendrl-Package-Installation-Reference) - official installation guide, end user scope
  * [Tendrl profiling](https://github.com/Tendrl/documentation/wiki/Tendrl-profiling) - dev details
  * and few wikipages for release notes and release installation docs - end user scope
* doc projects of particular components: TODO: list important component docs
* README files (TODO: list important README files)

So to sum this up:

 * documentation repository contains mostly dev information
 * very few sections from documentation repository are still up to date
 * nobody seems to care about documentation repository recently
 * there is no html build of documentation repository
 * documentation repository is not referenced from tendrl.org page
 * wikipages looks more useful and up to date compared to documentation repository (from both devel and user point of view)

The questions we need to answer before going forward:

 * Where should dev details go? To documentation repo or wiki?
 * Should documentation repo contain dev details?
 * Where should end user (storage admin using tendrl) docs go?
 * Do we want to maintain html builds of user/hight level dev documentation in the similar way ceph or gluster does?
 * Do we want to resurrect currently dead documentation repository or do we want to use wiki only?
