## Feb 13 2018
Agenda
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