## 12 June 2018
**Topics**

(1) Listing Gluster features Tendrl uses

(2) Latency of node detection related to import and reimport (dahorak)

(3) https://bugzilla.redhat.com/show_bug.cgi?id=1587804

(4) https://bugzilla.redhat.com/show_bug.cgi?id=1588650 (UX edge case)

## 05 June 2018
**Topics**

(1) Upgrade procedure

There are good amount changes in the etcd schema with respect to last release, most of them came in as part of the re-factoring activity. So if you just upgrade the bits, it will not work with the older schema. We need to write migration scripts which transform the older schema to newer one to work with newer bits. 

If we don't provide migration scripts, the procedure would be something like this(Needs to be verified):

 - Stop services
 - Clean up etcd
 - Run tendrl-ansible to upgrade the bits
 - Re-import the cluster

Since not many using older version, the decision is to provide manual upgrade path(as above) Or no upgrade path at all.

(2) https://github.com/Tendrl/documentation/wiki/Import-Failure-Modes
    
 - decided to go with option-2
 - suggest user to resolve the issue, unmanage, import again after failed import
 - the work flow described above is the only supported recovery procedure  which is supported and tested    
 - unmanage task is not run automatically, user needs to run it in explicit way (so that no log files or 
   configuration won't disappear without user being aware of it)
 - disable option to run import after failed import without unmanage, so that use can't avoid running 
   unmanage first


## 27 March 2018
**Topics**
* Discussed bugs and plans to address them:

(1) FQDN issue - plans to fix this. Estimated 1.5 week to do this work.  Reference: https://github.com/Tendrl/commons/issues/852.

(2) Race condition with etcd (serialization of objects) - modifications needed on how we save object.  Gluster is working on something like this.  They serialize the objects as they save them and unmarshall them during retrieval.  A lot of secondary issues result from these, e.g. repeated import.  Approx. 2 weeks to do this work.  Verification may take longer as all flows need to be verified.  Note: (1) and (2) can be worked on in parallel. Reference: https://github.com/Tendrl/commons/issues/885.

(3) 5 bugs/issues previously discussed — all fixed except FQDN issue and Short brick names issue outstanding. 
 dm-cache support not critical at this point.

Next proposed milestone date is 9 April 2018, need QE team to test these fixes.  Date is TBD.

(4) A cluster friendly name that user can specify
    - can we consider setting name (short name) only during Import as to do all the work for changing names after will be considerable.
    - would need to be visible everywhere (Tendrl UI, grafana dashboard)
    - Reference: https://github.com/Tendrl/ui/issues/656 and https://github.com/Tendrl/ui/issues/582
    - Proposal: https://github.com/Tendrl/commons/issues/888

(5) Prevent users from easily adding any Gluster cluster (like trusted or initial login/password from the cluster to be imported)
    - didn’t quite understand it as user has to be privileged to do this currently
    - not a priority at the moment, deferred for now
    - future discussion to ensure we understand use case properly

(6) Add /remove/maintenance mode for gluster nodes and or bricks - expansion already addressed
    - (1) nodes addition is supported as part of expand cluster
    - Future: no support for node removal (many manual Gluster steps today). Need to document and describe behavior.  E.g. if node is removed, the node will show up as down, volume would be shown down/unhealthy if brick is removed.
    - add / remove bricks support - tendrl detects this, i.e. when brick is removed, Tendrl detects that peer is detached.

(7) retention policy for carbon (/var/lib/carbon), disk size calculation, /var/lib/carbon growing from 50GB to 100GB
- retention policy and disk size calculation is already in the plan to get addressed
- how does preallocated changed from 50GB -> 100 GB?  Likely increases due to cluster expansion?  What triggers this?  @qe to investigate.
- Reference: https://github.com/Tendrl/monitoring-integration/issues/261 (disk size calculation)
 
(8) Gluster log grows a lot - @qe to file bug for tendrl services generating a lot of logs with default settings
- logs are from gluster (heal log, gluster events, bricks)
- tendrl is triggering these logs as we run the commands to perform queries.  Note: before import, there is no such logs, these logs grow after triggering self-heal.

(9) extend Gluster volume should not require uninstall and reinstall+import to recognize new capacity - out-of-band volume expansion is already supported.

(10) support removal of volume and not seeing it in the UI - needs to be verified.  See https://github.com/Tendrl/ui/issues/894#issuecomment-376519703 for more details.

(11) high hw requirements - already addressed but more performance and scale benchmarking work still outstanding
- serialization topic related to it (high network util)

(12) Docs recommendations
- Maintained and editable via GitHub to enable community contributions, and accessible from the Tendrl GitHub repo and Tendrl website
- Consider a https://github.com/Tendrl/tendrl-docs repo
- Should include the following:
```
- Getting Started & Install Guide
- Should include Performance and Sizing Guidelines
- Release Notes
- Administration / User Guide
- Troubleshooting Guide
- Developer’s / Contributor’s Guide
- Upgrade Guide
- Glossary
```

## 6 March 2018
**Topics**
* [Multiple Thresholds Bug](https://github.com/Tendrl/monitoring-integration/issues/346)
  - Do we change severity of old alert or clear the previous alert and generate a new alert?
  - Currently we generate warning alerts and not critical alerts
  - Cleaner to generate a new alert per threshold and easier to consume (recommended approach) vs. going back and updating the original alert with the newer threshold
  - Time lapse between clearing and creating new alert is seconds
  - How to handle throttling for emails and SNMP traps?  Extra intelligence to suppress certain emails needed later.  Address this issue later.
  - AI: Lubos T. to file an issue on the notifier issue

* [Expand Cluster handling](https://github.com/Tendrl/commons/issues/849)
  - Implemented but while testing but came across some corner cases
  - If user prepares all the new nodes and waits and does not do anything to setup Tendrl, should we wait for node-agent is installed?
  - Do we document it or do we not document and provide explicit way to import these new nodes?
  - Rohan does not think it should be automatic, i.e. you need some control over what’s installed without user acknowledging it, user should be given a chance there are 10 new nodes, and user authorizes the go ahead
  - Recommendation is to take conservative approach, and user has to “authorize” after tendrl-ansible is run
  - Provide 1 button to import the new nodes from cluster list and host list (if based on host list)
  - cluster would go to intermediate state to say it’s pending
  - Concern: what happens to cluster state when you don’t expand (i.e. user does not authorize the import)?
  - Concern: failure handling when the expansion fails for the added nodes?  Currently, user has to unmanage the entire cluster (though in the future it will be possible to unmanage partially or on a per node basis)
  - Note: No steps for user to reuse archive data currently
  - Question: what do we do with View Details (Cluster details) during expansion?
  - AI: UX design updates needed

* [Support for FQDN and IP for Gluster](https://github.com/Tendrl/commons/issues/852)
  - gluster allows you to peer probe fqdn, gluster supports short names for bricks, host + bricks uniquely identified via ip or fqdn
  - if we support both, it’s not big changes, need to simply figure out if fqdn available or not and also in central store and graphite
  - should be able to get this done by end of March 2018

AI: All participants/contributors asked to comment on the above topics in GitHub


## 20 February 2018
Topics
* Handling automatic unmanage as part of import flow will be too big an epic to perform, so we will keep them as 2 separate workflow.  
* Enabling/disabling volume profiling make take some time to perform and are handled as tasks -- see https://github.com/Tendrl/ui/issues/819#issuecomment-366959919.


## 13 February  2018
**Topics**
* Discussed scenarios for handling misconfigured cluster, unmanage cluster, and import cluster
* A misconfigured cluster occurs when the following is true:
```
- IsManaged = no
- The import job failed
- There is no error message
```
  This typically means that the user needs to do something in the CLI or tendrl-ansible to address the misconfiguration issue(s).  Upon resolving the issue(s) outside the UI, then only should an "Unmanage" (cleanup) action be performed prior to the "Import" Cluster.

* If cluster import fails or if Tendrl reports cluster as misconfigured, the Unmanage button should be enabled to allow for cleanup, i.e. Unmanage should work on failed to import and misconfigured cluster, in addition to managed cluster.
* UI should be able to handle unmanage first then import.
* For edge cases we identify, UI should not automatically unmanage and then import.
* For short term (upcoming Milestone 2), if cluster had a previous import failure or misconfiguration, then it will show a pop-up with some appropriate text to indicate that they've done some fixing in the CLI to resolve the issues first before Proceeding with Import or Cancel.
* Goal is to complete all this work mentioned (and in the Action Items) below in Milestone 3, though Unmanage will work in Milestone 2.


**Action Items**
* [Ju] new dialog (some issue with import, here are the logs, etc. click checkbox to cleanup)
* [Kanika, Neha, Anukush] Figure out persistent key value store in browser (cookies, html, etc.) to handle state of the tasks, i.e. UI fires an unmanage job, maintains state (unmanage job id) then fire import job (import job id)
* [Kanika, Neha, Ankush, Shubhendu] Enable Unmanage button if import fails or cluster misconfigured
* [Kanika, Shubhendu] Update Unmanage spec with changes mentioned
* [Shubhendu] In backend if there is a earlier import failure, allow the un-manage flow to go ahead


## Feb 6 2018


Reminder from last week's Architecture call was that we needed to clarify HW requirements for Tendrl deployment.  Decided to test on known sizes: 3 nodes, 6 nodes, 9 nodes
* [DONE] Email and Mojo page on cpu and memory based on perf team recommended


Daily call topics


(1) Unmanage cluster —> manage/import

    includes cleanup
    will not stop any agents
    cluster data is archived
    must maintain cluster entity ID so we don't orphaned archived data.
    Archived graphite data will not be brought back when reimported.  Can provide manual instructions.
    Notification to provide link to where the graphite data is archived.

(2) Delete cluster

    if you call delete cluster, then you have to run tendrl-ansible
    Issue: unable to stop agent currently

(3) Retention policy for etcd and graphite for a managed cluster

    what should duration be and what do we do with old data
    Current retention: graphite we keep up to 6 months.  For etcd, when it gets full, we currently say add disk; alerts: 2 days.
    graphite has an archive feature
    notifications (events): we don’t delete this data

Action Items

    * Jeff to join Gluster status team meetings to get Perf team to ensure test plan include Tendrl testing (last week action item)
    * Rohan will be updating Milestones for every repo
    * Need to provide recommendation on Retention Policy: https://github.com/Tendrl/commons/issues/819 -- input from Sayan
    * Nishanth to add Daily Call and all other Tendrl-related team discussions to the Tendrl Shared Calendar.
    * Nishanth needs to also add QE team to join daily call (meeting invite)
    * Ju needs to approve PR of some of specs (Note: Ju still only has Comment permissions and is unable to approve specs - pending Rohan to fix perms; Shubhendu also does not have perms too.)
    * Neha needs to update Concept B spec to include comments Ju mentioned on the spec
    * Need QE to begin testing the new Tendrl 1.5.5 release
        Filip B.: I will test it but I can not speak for others. There is currently only me and dahorak in the office today. ltrilety will probably come tommorow and mbukatov on Monday.
        Dahorak: Filip is looking at that release and I'm working on the CI part for CentOS CI. Martin is still on sick leave and Lubos have PTO (he should be back tomorrow). 