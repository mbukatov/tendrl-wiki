## Governance structure:
* "**Tendrl core**" - The Tendrl core team consists of ([Mrugesh Karnik](https://github.com/brainfunked), [Rohan Kanade](https://github.com/r0h4n), [Nishant Thomas](https://github.com/nthomas-redhat)).
* "**Reviewers**": reviewers are per component, all PRs on said component need sign-off from reviewers if not signed-off by "Tendrl core".
   * Reviewers:
       * [Jeff Applewhite](https://github.com/japplewhite) (For Components: specifications, dashboard, SDS metrics/monitoring dashboard and Alerting, documentation)
       * [Shubhendu Tripathi](https://github.com/shtripat) (For Components: ceph-integration)
       * [Darshan Narayana Murthy](https://github.com/nnDarshan) (For Components: gluster-integration)
       * [Anmol Babu](https://github.com/anmolbabu) (For Components: SDS metrics/monitoring dashboard and Alerting)
       * [Ju lim](https://github.com/julienlim) (For Components: dashboard, SDS metrics/monitoring dashboard and Alerting)
       * [Sankarshan Mukhopadhyay](https://github.com/sankarshanmukhopadhyay) (For Components: documentation)

* Tendrl core team manages the Tendrl GitHub organisation (includes management of contributors, component maintainers, org admins, CI)
* Decisions on Tendrl Architecture, road-map, code reviews, Tendrl project operations (CI, planning) require sign-off from the Tendrl core team.
* Core team domains:
   * [Mrugesh Karnik](https://github.com/brainfunked): project vision, architecture, roadmap, framework and component design, project-wide technical documentation
   * [Rohan Kanade](https://github.com/r0h4n): Management of code, Tendrl framework, operations eg: code reviews, documentation.
   * [Nishant Thomas](https://github.com/nthomas-redhat): Management of code, Automated testing, documentation, code reviews, Tendrl CI
* Core team responsibilities are based on their domains and are flexible.
* The core team members are the primary authority of their respective domains. However, any project wide decisions need to be agreed upon by all of the core members.
* The core team has the authority over delegation of the component ownership.
* Component owners are responsible for merging code on the respective component git repos.

## Code of conduct:
* The core team would ensure that their decisions are transparent and announced on the project’s mailing list.
* The core team would ensure that discussions, diverse opinions and contributions are encouraged, heard, evaluated and valid ones acted upon.
* The core team would ensure that the decisions made regarding the project and changes to the road-map would align with the project vision.

## Components/Repos:
* commons: {Owner: [Rohan Kanade](https://github.com/r0h4n), Repo: https://github.com/Tendrl/commons }
* node-agent: {Owner: [Rohan Kanade](https://github.com/r0h4n), Repo: https://github.com/Tendrl/node-agent }
* api: {Owner: [Anup Nivargi](https://github.com/anivargi), Repo: https://github.com/Tendrl/api }
* python-tendrlclient: {Owner: [Rohan Kanade](https://github.com/r0h4n), Repo: https://github.com/Tendrl/python-tendrlclient }
* ruby-tendrlclient: {Owner: [Anup Nivargi](https://github.com/anivargi), Repo: https://github.com/Tendrl/tendrl-ruby }
* ceph-integration: {Owner: [Rohan Kanade](https://github.com/r0h4n), Repo: https://github.com/Tendrl/ceph-integration }
* gluster-integration: {Owner: [Rohan Kanade](https://github.com/r0h4n), Repo: https://github.com/Tendrl/gluster-integration }
* python-gdeploy : {Owner: [Darshan Narayana Murthy](https://github.com/nnDarshan), Repo: https://github.com/Tendrl/python-gdeploy }
* SDS metrics/monitoring dashboard and Alerting: {Owner: [Anup Nivargi](https://github.com/anivargi), Repo: TBD}
* dashboard: {Owner: [Ankush Behl](https://github.com/cloudbehl), [Neha Gupta](https://github.com/gnehapk), Repo: https://github.com/Tendrl/dashboard }
* tendrl.org: {Owner: Tendrl core, [Jeff Applewhite](https://github.com/japplewhite), Repo: https://github.com/Tendrl/tendrl.github.io }
* tendrl-ansible: {Owner: [Martin Bukatovic](https://github.com/mbukatov), Repo: https://github.com/Tendrl/tendrl-ansible }
* documentation: {Owner: Tendrl core, [Martin Bukatovic](https://github.com/mbukatov), Repo: https://github.com/tendrl/documentation }
* specifications: {Owner: Tendrl core, Repo: https://github.com/Tendrl/specifications }

## Unsupported repos (to be moved out of github.com/Tendrl unless adapted to Tendrl repo best practices)
* https://github.com/Tendrl/usmqe-tests (Need repo name change as "tendrl-tests")
* https://github.com/Tendrl/usmqe-centos-ci (Need repo name change as "tendrl-centos-ci")
* https://github.com/Tendrl/usmqe-setup (Needs to be merged with https://github.com/Tendrl/tendrl-ansible)
* https://github.com/Tendrl/ui (current and future content need to be sent as PRs to Tendrl/dashboard and the upcoming metrics/monitoring dashboard)
