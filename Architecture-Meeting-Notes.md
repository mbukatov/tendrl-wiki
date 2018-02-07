# Feb 6 2018


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