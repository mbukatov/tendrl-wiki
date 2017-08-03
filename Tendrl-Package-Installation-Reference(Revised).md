## Server Installation

The following procedure outlines the procedure to install tendrl server components

1. Install CentOS 7.3

2. Enable the following repositories

   ```
   wget https://copr.fedorainfracloud.org/coprs/tendrl/release/repo/epel-7/tendrl-release-epel-7.repo
   wget https://copr.fedorainfracloud.org/coprs/tendrl/dependencies/repo/epel-7/tendrl-dependencies-epel-7.repo
   cp tendrl-*.repo /etc/yum.repos.d
   yum install epel-release
   ```
   
3. Install Etcd

   ```
   yum install etcd
   ```

4. Configure etcd

   Edit the below etcd configurations for allowing the clients to connect to the etcd server

   Open `/etc/etcd/etcd.conf` and update:

   * `ETCD_LISTEN_CLIENT_URLS="http://<IP of etcd server>:2379"`
   * `ETCD_ADVERTISE_CLIENT_URLS="http://<IP of etcd server>:2379"`

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
    etcd_connection = <IP of etcd server>
    graphite_host = <IP of Graphite Server>
    graphite_port = <Port of Graphite Server>
    ```

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
    yum install tendrl-ui`
    ```

13. Install Performance Monitoring

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

16. Restart httpd

    ```
    systemctl restart httpd
    ```

17. Configure monitoring-integration

    Open `/etc/tendrl/monitoring-integration/monitoring-integration.conf.yaml`
    and update:
   
    ```
    datasource_host = <IP of graphite server>
    ```

18. Enable and start performance monitoring service

    ```
    systemctl enable tendrl-monitoring-integration
    systemctl start tendrl-monitoring-integration
    ```   

19. Open the following URL in the browser

    ```
    http://<IP of the server>
    ```


## Storage Node Installation

Note: If you are configuring the v 1.2.2 release, please refer to the sequence of steps at https://github.com/Tendrl/documentation/wiki/Tendrl-release-v1.2.2 This document has instructions specific to tags to be appended to the YAML files for tendrl-node-agent

1. Install CentOS 7.3

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
   graphite_port = <Port of Graphite Server>
   ```

5. Enable and start Node Agent

   ```
   systemctl enable tendrl-node-agent
   systemctl start tendrl-node-agent
   ```

## SELinux Configuration

   Tendrl does not currently support running on SELinux enabled systems. In case there are problems
   running tendrl on such systems, please set SELinux to 'Permissive' mode.

## Firewall Configuration

Tendrl does not currently support running on firewall enabled system as the firewall rules are under development. Hence it is recommended to disable the firewalld on server/storage nodes
 
```
service firewalld stop
systemctl disable firewalld
iptables --flush
```
