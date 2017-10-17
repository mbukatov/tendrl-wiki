## Tendrl (Monitoring Server) System Requirements
The server hosting tendrl-api/central_store should have minimum 12 GB of memory and 4 VCPUs (or equivalent)(due to alerts, logs being stored on this node)

## Supported SDS
* Tendrl requires **Gluster>=3.12.0**

## SELinux Configuration

Tendrl support running on SELinux enabled systems. Procedure to install the policies a are mentioned below.

**Server**
* yum install carbon-selinux
* yum install tendrl-grafana-selinux
* yum install tendrl-server-selinux

**Storage Nodes**
*  yum install tendrl-collectd-selinux
*  yum install tendrl-node-selinux

**Enabling selinux is optional and not tested completely (WARNING)**

In case there are problems running tendrl on such systems, please set SELinux to
'permissive' mode:

```
Modify /etc/selinux/config, so that SELINUX=permissive
reboot 
```

## Firewall Configuration

Tendrl does not currently support running on firewall enabled system as the
firewall rules are under development. Hence it is recommended to disable the
firewalld on server/storage nodes
 
```
service firewalld stop
systemctl disable firewalld
iptables --flush
```

## NTP

Make sure you keep time synchronized on all storage machines and Tendrl server.
When you install Tendrl on machines with already existing storage cluster, an ntp daemon (such as chrony or ntpd) is usually already configured because it's part of the storage cluster installation.

```
yum install ntpdate
ntpdate <ntp_host>
```

## Python DNS 

Make sure Python DNS is installed on Tendrl server.

```
yum install python-dns 
```

## Ansible

Make sure Ansible is installed on Tendrl server and all storage nodes, and that the Tendrl server can ssh to each of the storage nodes.

```
yum install epel-release
yum install ansible
```

## Https Configuration

Please refer to  https://github.com/Tendrl/documentation/wiki/Enabling-Https-on-tendrl-server



## Server Installation (Tendrl-Ansible)
To install (tendrl server, tendrl agents on storage nodes) via tendrl-ansible:

1) Follow readme at https://github.com/Tendrl/tendrl-ansible/tree/release/1.5.3


## Server Installation (Manual)

The following procedure outlines the procedure to install tendrl server components manually

1. Install CentOS 7.3

2. Enable the following repositories

   ```
   wget https://copr.fedorainfracloud.org/coprs/tendrl/release/repo/epel-7/tendrl-release-epel-7.repo
   wget https://copr.fedorainfracloud.org/coprs/tendrl/dependencies/repo/epel-7/tendrl-dependencies-epel-7.repo
   cp tendrl-*.repo /etc/yum.repos.d
   yum install epel-release
   ```
   Add Grafana repo as per the instructions at  http://docs.grafana.org/installation/rpm/#install-via-yum-repository
   
3. Install Etcd

   ```
   yum install etcd
   ```

4. Configure etcd

   Edit the below etcd configurations for allowing the clients to connect to the etcd server

   Open `/etc/etcd/etcd.conf` and update:

   * `ETCD_LISTEN_CLIENT_URLS="http://<FQDN of etcd server>:2379"`
   * `ETCD_ADVERTISE_CLIENT_URLS="http://<FQDN of etcd server>:2379"`

   As a value for *etcd server FQDN*, use some *public FQDN address of the
   tendrl server machine* (which is the server you are installing etcd on right
   now).

5. Enable and start the etcd service

   ```
   systemctl enable etcd
   systemctl start etcd
   ```
   * ** To run secure ETCD (SSL/TLS based encryption and auth), please refer https://github.com/Tendrl/documentation/wiki/Tendrl-with-a-secure-etcd-cluster**
6. Install Node Agent

    ```
    yum install tendrl-node-agent
    ```

7. Configure Node Agent

    Edit the below configurations for connecting to the etcd server

    Open `/etc/tendrl/node-agent/node-agent.conf.yaml` and update:

    ```
    etcd_connection: <FQDN of etcd server>
    graphite_host: <FQDN of Graphite Server>
    ```

    Note that:

    * we configured *FQDN address of etcd server* just few steps ago
    * a safe default value for *FQDN address of graphite* is the same one we
      use for etcd here (this guide places both services on tendrl server
      machine)
    * graphite stack is installed later as a dependency of
      `tendrl-monitoring-integration` rpm package
    * you should not reconfigure `graphite_port` in this config file

    Additional details (useful when you are familiar with graphite stack):

    * this guide doesn't include steps to reconfigure any component for
      graphite stack so that we can assume that default configuration is used
    * `graphite_host` refers to `carbon-cache` service, which is configured
      in `/etc/carbon/carbon.conf` config file

8. Enable and start Node Agent

    ```
    systemctl enable tendrl-node-agent
    systemctl start tendrl-node-agent
    ```

9. Install tendrl API

   ```
   yum install tendrl-api
   ```

10. Configure tendrl API

    Edit configuration file `/etc/tendrl/etcd.yml` for connecting to the etcd
    server and update `:production:` section:

    ```
    :production:
        :base_key: ''
        :host: '<FQDN of etcd server>'
        :port: 2379
        :user_name: ''
        :password: ''
    ```
    Then create the admin user:

    ```
    cd /usr/share/tendrl-api
    RACK_ENV=production rake etcd:load_admin
    ```

    Note that the default password of the admin user will be shown in output of
    rake command.

11. Enable and start API service

    ```
    systemctl enable tendrl-api
    systemctl start tendrl-api
    ```

12. Install tendrl ui

    ```
    yum install tendrl-ui
    ```

13. Install Monitoring Integration

    ```
    yum install tendrl-monitoring-integration
    ```

13. Init graphite-db

    ```
    /usr/lib/python2.7/site-packages/graphite/manage.py syncdb --noinput
    chown apache:apache /var/lib/graphite-web/graphite.db
    ```

15. Enable and start carbon-cache service

    ```
    systemctl enable carbon-cache
    systemctl start carbon-cache
    ```

16. Configure grafana service

    Open `/etc/sysconfig/grafana-server` and update:

    ```
    CONF_DIR: /etc/tendrl/monitoring-integration/grafana/
    CONF_FILE: /etc/tendrl/monitoring-integration/grafana/grafana.ini
    ```

17. Enable and start grafana service

    ```
    systemctl daemon-reload
    systemctl enable grafana-server.service
    systemctl start grafana-server
    ```

    Note that the 1st step here (daemon reload) is actually needed as is a
    workaround for upstream grafana rpm package we are using right now:

    ```
          Installing : grafana-4.4.3-1.x86_64
    ### NOT starting on installation, please execute the following statements to configure grafana to start automatically using systemd
     sudo /bin/systemctl daemon-reload
     sudo /bin/systemctl enable grafana-server.service
    ### You can start grafana-server by executing
     sudo /bin/systemctl start grafana-server.service 
    ```

18. Configure monitoring-integration

    Modify `/etc/tendrl/monitoring-integration/monitoring-integration.conf.yaml`:
   
    ```
    datasource_host: <FQDN of graphite server>
    etcd_connection: <FQDN of etcd server>
    ```

19. Enable and start monitoring-integration

    ```
    systemctl enable tendrl-monitoring-integration
    systemctl start tendrl-monitoring-integration
    ```

20. Enable and start httpd

    ```
    systemctl enable httpd
    systemctl start httpd

    ```

21. Install Notifier

    ```
    yum install tendrl-notifier
    ```

22. Configure notifier

    Open `/etc/tendrl/notifier/notifier.conf.yaml`
    and update:
   
    ```
    etcd_connection: <FQDN of etcd server>
    etcd_port: <Port of etcd server>
    ```

23. Configure email/snmp source::

    ```
    Email:
    Open /etc/tendrl/notifier/email.conf.yaml
   
    update -->

    email_id = <The sender email id>

    email_smtp_server = <The smtp server>

    email_smtp_port = <The smtp port>


    Note: If SMTP server supports only authenticated email, 
    follow the template as in: /etc/tendrl/notifier/email_auth.conf.yaml.sample
          And accordingly enable the following:

        auth = <ssl/tls>

        email_pass = <password corresponding to email_id for authenticating to smtp server>

    
    SNMP:
    Open /etc/tendrl/notifier/snmp.conf.yaml

    For v2_endpoint:
    # For more hosts you can add more entry with endpoint2, endpoint3, etc
    endpoint1:
    # Name or IP address of the remote SNMP host.
    host_ip: <Receiving machine ip>
    community: <community name>

    # In receiving host machine:
    yum install net-snmp
    open file snmptrapd.conf
    # write below line inside file
    disableAuthorization yes
    # Run command
    snmptrapd -f -Lo -c snmptrapd.conf

    For v3_endpoint:
    # For more hosts you can add more entry with endpoint2, endpoint3, etc
    endpoint1:

    # Name or IP address of the remote SNMP host.
    host_ip: <Receiving machine ip>
    # Name of the user on the host that connects to the agent.
    username: <Username of receiver>
    # Enables the agent to receive packets from the host.
    auth_key: <md5 password>
    #  The private user password
    priv_key: <des password>

    # In receiving host machine:
    yum install net-snmp
    open file snmptrapd.conf
    # write below line inside file
    authUser log <username of receiver>
    createUser -e 8000000001020304 <user name of receiver> MD5 <md5 password> DES <des password>
    # Run command
    snmptrapd -f -Lo -c snmptrapd.conf

    ```

24. Enable and start notifier service::

    ```
    systemctl enable tendrl-notifier
    systemctl start tendrl-notifier

    ```
   
25. Open the following URL in the browser

    ```
    http://<FQDN of the server>
    ```


## Storage Node Installation

1. Install CentOS 7.3 and Gluster. Ensure all the participating nodes in the Gluster cluster are peer probed (i.e. present in gluster trusted storage pool), only after which tendrl-node-agent should be installed on all nodes, without peer probe, the node wont be detected by tendrl as a gluster node.

2. Enable the following repositories

   ```
   wget https://copr.fedorainfracloud.org/coprs/tendrl/release/repo/epel-7/tendrl-release-epel-7.repo
   wget https://copr.fedorainfracloud.org/coprs/tendrl/dependencies/repo/epel-7/tendrl-dependencies-epel-7.repo
   cp tendrl-*.repo /etc/yum.repos.d
   yum install epel-release
   ```

3. Install Node Agent

   ```
   yum install tendrl-node-agent
   ```

4. Configure Node Agent

   Edit the below configurations for connecting to the etcd server

   Open `/etc/tendrl/node-agent/node-agent.conf.yaml` and update:

   ```
   etcd_connection = <FQDN of etcd server>
   graphite_host = <FQDN of Graphite Server>
   ```

5. Enable and start Node Agent

   ```
   systemctl enable tendrl-node-agent
   systemctl start tendrl-node-agent
   ```