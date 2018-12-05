## 1. Etcd SSL configuration:

For enabling SSL for etcd follow 
[Etcd SSL enabling using tendrl-ansible](https://github.com/GowthamShanmugam/documentation/wiki/Etcd-SSL-configuration-using-tendrl-ansible)

## 2. Enabling HTTPS for Tendrl-UI, Tendrl-API and Grafana:

### Scope
This is achieved through apache redirection configuration and this configuration enables two scenarios:
  - HTTPS delivery for the tendrl-API, tendrl-UI, and grafana on an IP address on the host.
  - Automatic redirect for all HTTP requests to HTTPS, for tendrl-API,  tendrl-UI, and grafana.

## Pre-requisites:

* `mod_ssl` package installed and the default configurations left unmodified.
* The default SSL certificate and key paths are used, this certificate would be a self-signed certificate.

## Limitations

This configuration file avoids modification to any of the configuration files installed by system packages. As such, this configuration file can only serve the scenarios where the requests are served over a specific IP. If this is undesirable, the `VirtualHost` for `_default_:443` needs to be commented out in `/etc/httpd/conf.d/ssl.conf` (which is installed by the `mod_ssl` package) and the `%ssl_virtualhost_ip%` in both these files needs to be changed to `_default_`.

Please refer to the [apache wiki](https://wiki.apache.org/httpd/NameBasedSSLVHosts) for more details.

## Deployment Instructions

### https support over a specific IP with no redirect

copy (NOT move) the `/etc/httpd/conf.d/tendrl-ssl.conf.sample` file without the `.sample` extension. Make the following changes to this file:

* Replace `%ssl_virtualhost_ip%` with the correct IP.
* Adjust `ServerName`.

Thereafter, check if the configuration is valid using `apachectl -t` and reload httpd using `systemctl reload httpd.service`.

### Automatic redirect of all http urls to https

After following the steps to enable https, as listed above, update the `/etc/httpd/conf.d/tendrl.conf` file  as follows:

* Replace `%ssl_virtualhost_ip%` with the IP used in the SSL configuration file.
* Un-comment the line which has the `Redirect` rule.
* Comment out the lines which have the `DocumentRoot`, `ProxyPass` and `ProxyPassReverse` directives.

Thereafter, check if the configuration is valid using `apachectl -t` and reload httpd using `systemctl reload httpd.service`.