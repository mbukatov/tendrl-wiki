## Tendrl supports etcd's TLS-based security model, which supports the encryption (and authentication) of traffic between Tendrl services and the etcd cluster.


### To enable etcd TLS support:
- Follow the instructions in the [etcd security guide](https://coreos.com/etcd/docs/latest/op-guide/security.html) to create a certificate authority (i.e CA cert file) and enable TLS in etcd. We recommend enabling both client and peer authentication. This will enable security between Tendrl and etcd as well as between different nodes in the etcd cluster.
- Issue a private key and client certificate for each Tendrl storage node and the Tendrl server, Alternatively you can also issue a private key and client certificate for each individual Tendrl service. Follow [Generating self-signed certificates](https://coreos.com/os/docs/latest/generate-self-signed-certificates.html) or see [Example](https://github.com/coreos/etcd/tree/v3.2.7/hack/tls-setup)
- On each Tendrl managed storage node and on the Tendrl server, Put the PEM-encoded private key and client/ca certificates in a secure place that is only accessible by the user (root) that Tendrl will run as.
- Modify service config (eg: /etc/tendrl/node-agent/node-agent.conf.yaml) for all Tendrl services on storage nodes and Tendrl server by adding below items
    * `etcd_ca_cert_file: /path/to/ca_cert_file.pem`
    * `etcd_cert_file: /path/to/client_cert_file.pem`
    * `etcd_key_file: /path/to/client_key_file.pem`
- Open `/etc/etcd/etcd.conf` and update (more etcd ssl related options available, check [examples](https://coreos.com/etcd/docs/latest/op-guide/security.html)
    * `ETCD_CLIENT_CERT_AUTH="true"`
    * `ETCD_TRUSTED_CA_FILE="/path/to/ca_cert_file.pem"`
    * `ETCD_CERT_FILE="/path/to/server_cert_file.pem"`
    * `ETCD_KEY_FILE="/path/to/server_key_file.pem"`
    * `ETCD_LISTEN_CLIENT_URLS="https://<hostname_or_fqdn of etcd server>:2379"`
    * `ETCD_ADVERTISE_CLIENT_URLS="https://<hostname_or_fqdn of etcd server>:2379"`
- Restart etcd service and restart all Tendrl services on all storage nodes and Tendrl server
