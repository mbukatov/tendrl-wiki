This is overview of pending work related to SSL configuration for Tendrl.

## What are the use cases?

* Secure all direct user facing interfaces of Tendrl server machine (tendrl web, grafana web).
* Secure all remaining services running on Tendrl server machine (etcd, carbon, ...).
* Secure all communication channels within storage cluster (etcd, carbon, ...).

By **secure** we mean both:

* TLS based encryption (aka SSL) of the communication channel
* authentication of all encrypted channels (there is no point in encrypting communication channel when one can open one for themselves without authentication)

Ports opened on Tendrl server:

* 2379/tcp etcd (could be encrypted and authenticated via ssl, which is documented)
* 2003/tcp carbon
* ~10080/tcp~ graphite is no longer available via this port from the outside
* ~9292/tcp~ puma (tendrl-api) is now available via httpd: `http(s)://hostname/api/1.0/`
* ~3000/tcp~ Grafana dashboard is now available via httpd: `http(s)://hostname/grafana/`
* 8789/tcp webhook receiver
* 80/tcp (apache web server, hosting Tendrl web, API and Grafana dashboard)

## Recent or Planned features related to SSL Configuration

### Make grafana available via http instead of using dedicated port 3000/tcp

* https://github.com/Tendrl/ui/pull/1074
* https://github.com/Tendrl/api/pull/447
* https://github.com/Tendrl/monitoring-integration/pull/578
* https://github.com/Tendrl/tendrl-ansible/issues/127

## What needs to be done

List of items to consider:

* [ ] figure out which components need to use SSL:
   - web browser -> tendrl (served via httpd)
   - web browser -> grafana (served via httpd)
   - web browser -> graphite:
      - [x] Do we want to have graphite web available? The answer is no (the port has been recently disabled).
      - [x] ~If yes, we need to proxy it through httpd as well.~ Not applicable (see above).
   - various tendrl components -> graphite
   - various tendrl components -> etcd (TLS client-server based authentication)
      - [x] already documented
      - [x] partially implemented via tendrl-ansible (when users generate TLS CA files themselves)
      - [ ] deployment of default Tendrl CA for TLS client-server based authentication
* [ ] implement tendrl - grafana authentication
* [ ] additional code changes may be needed (excluding tendrl-ansible), eg: some hardcoded http urls
* [ ] how to configure it (initial dev docs)
* [ ] how to implement default via tendrl-ansible
* [ ] document when default CA deployment provided by tendrl-ansible is a good idea

## Minimal SSL support

Idea here is to document how to setup SSL for HTTP in a minimal way, without resolving all items as listed in *What needs to be done* section. Such SSL support is incomplete.

Overview of this ssl minimal solution:

* Tendrl web, API and Grafana dashboard, which are provided by apache server, will be secured with SSL by reconfiguration of apache.
* Tendrl contains sample configuration files for apache to simplify the SSL setup.
* Setup of ssl is described in documentation wikipage [Enabling Https on tendrl server](https://github.com/Tendrl/documentation/wiki/Enabling-Https-on-tendrl-server)
* Setup of ssl configuration for Tendrl web, API and Grafana dashboard is not part of tendrl-ansible.
* Nothing else will be secured or restricted compared to current setup.
* Resolve all issues in Tendrl itself which prevents this minimal ssl use case to work properly (either by breaking ssl somehow or introcuding regressions in tendrl functionality).

This is drafted in current documentation: [Enabling Https on tendrl server](https://github.com/Tendrl/documentation/wiki/Enabling-Https-on-tendrl-server)

## Previous (now deprecated) work

Original plan was to have ssl supported and included in tendrl-ansible as an option, but this haven't been done because of several issues with the design. See *What needs to be done* section above or issues and pull requests linked below:

* [Original idea described in the documentation wikipage *Enabling Https on tendrl server*](https://github.com/Tendrl/documentation/wiki/Enabling-Https-on-tendrl-server/ec23602bf63a744c307bfcb336b46b9ae31384a0)
* Previous work done in tendrl-ansible:
    * https://github.com/Tendrl/tendrl-ansible/issues/30
    * https://github.com/Tendrl/tendrl-ansible/pull/46