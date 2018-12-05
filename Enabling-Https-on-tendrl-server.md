Tendrl supports etcd’s TLS-based security model which supports authentication and encryption of traffic between etcd and tendrl components.

By default, etcd functions without authentication and encryption but it is recommended to use TLS authentication for client-server encryption.

### Prerequisites
The tendrl-ansible installation does not generate and deploy encryption certificates and keys. To configure etcd TLS client-server authentication, generate and deploy encryption certificates on all the nodes of the cluster before executing tendrl-ansible.

Before setting up the Transport Layer Security (TLS ) encryption, ensure the following encryption component is generated:

### Private Keys
Generate a private key and a client certificate for each storage node and the tendrl server. For more information, see [Generating self-signed certificates](https://coreos.com/os/docs/latest/generate-self-signed-certificates.html). On each tendrl managed storage node, and on the tendrl server, place the PEM-encoded private key and the client/CA certificates in a secure place that is only accessible by the tendrl server’s root user.

### Configuring TLS Encryption
After generating and placing the TLS certificate files in the preferred directory, update the value of the Ansible variables in the inventory file with the respective file paths of the certificate files.

In the **inventory file**, add and modify the etcd TLS variables under **[all:vars]**.

* **etcd_tls_client_auth**= this variable is to enable or disable TLS authentication.

* **etcd_cert_file**= certificate used for SSL/TLS connections to etcd. When this option is turned on, advertise-
client-urls can use the HTTPS schema.

* **etcd_key_file**= key for the certificate which must be unencrypted.

* **etcd_trusted_ca_file**= the trusted Certificate Authority.

#### Configuring TLS
1. Open the **inventory file** playbook file under **[all:vars]**.

2. Set the value for **etcd_tls_client_auth** variable to **True** for both the Ansible roles: 
   tendrl_server and gluster_servers. By default, the value of this variable is False.

3. Edit the file path for the **etcd_cert_file** variable as per required. The default value is: 
   /etc/pki/tls/certs/etcd.crt

4. Edit the file path for **etcd_key_file** variable as per required. The default value is: 
   /etc/pki/tls/private/etcd.key

5. Edit the file path for the **etcd_trusted_ca_file** variable. The default value is: 
   /etc/pki/tls/certs/ca-etcd.crt

6. Continue the tendrl installation process by following the [Tendrl Installation](https://github.com/Tendrl/tendrl-ansible)