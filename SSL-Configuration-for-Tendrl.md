This is overview of pending work related to SSL configuration for Tendrl.

## What are the use cases?

* Secure all direct user facing interfaces of Tendrl server machine (tendrl web, grafana)
* Secure all communications within storage cluster (etcd, carbon, ...)

Since Tendrl server machine hosts all Tendrl services listening on some tcp port, we need to secure all of them.

## What needs to be done

* [ ] figure out which components need to use ssl:
   - web browser - tendrl (via httpd)
   - web browser - grafana (via httpd)
   - web browser - graphite
   - graphite - various tendrl components
   - etcd - various tendrl components (already partially implemented via tendrl-ansible)
* [ ] code changes needed?
* [ ] how to configure it (initial dev docs)
* [ ] how to implement default via tendrl-ansible

## Previous now deprecated work

Previous guide:

* https://github.com/Tendrl/documentation/wiki/Enabling-Https-on-tendrl-server

Previous work done in tendrl-ansible:

* https://github.com/Tendrl/tendrl-ansible/issues/30
* https://github.com/Tendrl/tendrl-ansible/pull/46

