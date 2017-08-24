## SELinux Configuration

Tendrl does not currently support running on SELinux enabled systems. In case
there are problems running tendrl on such systems, please set SELinux to
'Permissive' mode.

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

## Server Installation (Ansible)
To install (tendrl server, tendrl agents on storage nodes) via tendrl-ansible

1) Follow readme at https://github.com/Tendrl/tendrl-ansible/tree/release/1.5.1

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

   * `ETCD_LISTEN_CLIENT_URLS="http://<IP of etcd server>:2379"`
   * `ETCD_ADVERTISE_CLIENT_URLS="http://<IP of etcd server>:2379"`

   As a value for *etcd server ip address*, use some *public ip address of the
   tendrl server machine* (which is the server you are installing etcd on right
   now).

5. Enable and start the etcd service

   ```
   systemctl enable etcd
   systemctl start etcd
   ```

6. Install Node Agent

    ```
    yum install tendrl-node-agent
    ```

7. Configure Node Agent

    Edit the below configurations for connecting to the etcd server

    Open `/etc/tendrl/node-agent/node-agent.conf.yaml` and update:

    ```
    etcd_connection: <IP of etcd server>
    graphite_host: <IP of Graphite Server>
    ```

    Note that:

    * we configured *ip address of etcd server* just few steps ago
    * a safe default value for *ip address of graphite* is the same one we
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
        :host: '<IP of etcd server>'
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

16. Enable and start grafana service

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

17. Configure monitoring-integration

    Open `/etc/tendrl/monitoring-integration/monitoring-integration.conf.yaml`
    and update:
   
    ```
    datasource_host: <IP of graphite server>
    etcd_connection: <IP of etcd server>
    ```

18. Enable and start monitoring-integration

    ```
    systemctl enable tendrl-monitoring-integration
    systemctl start tendrl-monitoring-integration
    ```

19. Enable and start httpd

    ```
    systemctl enable httpd
    systemctl start httpd
    ```

20. Open the following URL in the browser

    ```
    http://<IP of the server>
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
   etcd_connection = <IP of etcd server>
   graphite_host = <IP of Graphite Server>
   ```

5. Enable and start Node Agent

   ```
   systemctl enable tendrl-node-agent
   systemctl start tendrl-node-agent
   ```
