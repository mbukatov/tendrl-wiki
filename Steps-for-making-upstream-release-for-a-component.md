* Make sure all commits in all the PRs targeted for release have acks in bugzilla and are merged in the master
* Create a branch(for eg. rel_prep/1.6.3-4) and do all the release preparation change
  * Update release(if required) and version in the Makefile
  * Update release(if required), version and change log in the spec file for the respective component  
    You can refer https://github.com/Tendrl/ui/pull/990/files  
* Sent a PR from rel_prep branch to master.
* Once merged, create a another PR for merging backporting changes from master to release branch(Eg. https://github.com/Tendrl/ui/pull/991). You can directly create a PR from UI if all the changes merged to master are targeted for release else you need to
  * Create a branch
  * Take all the changes there.
  * And then sent a PR to release branch
* Once the backport PR is merged, you can announce the upstream release for the component

