This is overview of pending work related to SSL configuration for Tendrl.

## What needs to be secured via SSL

* etcd (already possible)

## What needs to be done

* [ ] figure out which components need to use ssl:
   - tendrl (via httpd)
   - graphite
   - grafana
   - etcd
* [ ] code changes needed?
* [ ] how to configure it (initial dev docs)
* [ ] how to implement default via tendrl-ansible

## Previous now deprecated work

Previous guide:

* https://github.com/Tendrl/documentation/wiki/Enabling-Https-on-tendrl-server

Previous work done in tendrl-ansible:

* https://github.com/Tendrl/tendrl-ansible/issues/30
* https://github.com/Tendrl/tendrl-ansible/pull/46

