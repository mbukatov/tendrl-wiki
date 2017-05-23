## Server Installation

The following procedure outlines the procedure to install tendrl server components

1. Install CentOS 7.3

2. Enable the following repositories

   `wget https://copr.fedorainfracloud.org/coprs/tendrl/release/repo/epel-7/tendrl-release-epel-7.repo`
   
   `wget https://copr.fedorainfracloud.org/coprs/tendrl/dependencies/repo/epel-7/tendrl-dependencies-epel-7.repo`

    `cp tendrl-*.repo /etc/yum.repos.d`

    `yum install epel-release`
    
    configure the ceph-installer repo (as made available by the project).

3. Install Etcd

   `yum install etcd`

4. Configure etcd

   Edit the below etcd configurations for allowing the clients to connect to the etcd server

   `Open /etc/etcd/etcd.conf`

   `Update ETCD_LISTEN_CLIENT_URLS="http://<IP of etcd server>:2379"`
  
   `Update ETCD_ADVERTISE_CLIENT_URLS="http://<IP of etcd server>:2379"`

5. Enable and start the etcd service

   `systemctl enable etcd`

   `systemctl start etcd`

6. Install tendrl API

   `yum install tendrl-api`

7. Configure tendrl API

   Edit the below configurations for connecting to the etcd server

   `Open /etc/tendrl/etcd.yml`
   
   `update -->`
   
      `:production:`

          :base_key: ''

          :host: '<IP of etcd server>'

          :port: 2379

          :user_name: ''

          :password: ''

8. Enable and start API service

   `systemctl enable tendrl-apid`

   `systemctl start tendrl-apid`

9. Install Node Agent

    `yum install tendrl-node-agent`

10. Configure Node Agent

    Edit the below configurations for connecting to the etcd server

    `Open /etc/tendrl/node-agent/node-agent.conf.yaml`

    `Update →`

    `etcd_connection = <IP of etcd server>`

    `Add a new tag under tags:`

    `provisioner/ceph`

11. Enable and start Node Agent

    `systemctl enable tendrl-node-agent`

    `systemctl start tendrl-node-agent`

12. Install ceph installer

    `yum install ceph-installer`

13. Install tendrl dashboard

    `yum install tendrl-dashboard`

14. Restart httpd
   
    `systemctl restart httpd`

15. Disable Firewall

    `service firewalld stop`

    `systemctl disable firewalld`

    `iptables --flush`

16. Open the following URL in the browser

    `http://<IP of the server>`

## Performance Monitoring installation

    Note:
    This can be installed either on

        1. Node where etcd is installed
                or
        2. New node
           Please follow steps 1 and 2 above in addition to below steps

1. Install Performance Monitoring yum install tendrl-performance-monitoring

   `yum install tendrl-performance-monitoring`

2. Init graphite-db

   `/usr/lib/python2.7/site-packages/graphite/manage.py syncdb --noinput`

   `chown apache:apache /var/lib/graphite-web/graphite.db`

3. Enable and start carbon-cache service

   `systemctl enable carbon-cache`

   `systemctl start carbon-cache`

4. Restart httpd

   `systemctl restart httpd`

5. Configure performance monitoring

    `Open /etc/tendrl/performance-monitoring/performance-monitoring.conf.yaml`
   
    `update -->`

    `etcd_connection = <IP of etcd server>`

    `time_series_db_server = <IP of current node>`

    `api_server_addr = <IP of current node>`

6. Enable and start performance monitoring service

   `systemctl enable tendrl-performance-monitoring`

   `systemctl start tendrl-performance-monitoring`
     

## Storage Node Installation

Note: If you are configuring the v 1.2.2 release, please refer to the sequence of steps at https://github.com/Tendrl/documentation/wiki/Tendrl-release-v1.2.2 This document has instructions specific to tags to be appended to the YAML files for tendrl-node-agent

1. Install CentOS 7.3

2. Enable the following repositories

   `wget https://copr.fedorainfracloud.org/coprs/tendrl/tendrl/repo/epel-7/tendrl-tendrl-epel-7.repo`

   `wget https://copr.fedorainfracloud.org/coprs/tendrl/dependencies/repo/epel-7/tendrl-dependencies-epel-7.repo`

   `cp tendrl-*.repo /etc/yum.repos.d`

   `yum install epel-release`

    configure the ceph-mon, ceph-osd repo (as made available by the project and required only for ceph nodes). Make sure to disable the gpgcheck if the keys are not setup properly

    Configure the gdeploy repo - https://copr.fedorainfracloud.org/coprs/sac/gdeploy/repo/epel-7/sac-gdeploy-epel-7.repo - required only for gluster nodes


3. Install Node Agent

   `yum install tendrl-node-agent`

4. Configure Node Agent

   Edit the below configurations for connecting to the etcd server

   `Open /etc/tendrl/node-agent/node-agent.conf.yaml`

   `Update →`

   `etcd_connection = <IP of etcd server>`
   
   `Add a new tag under tags(Only for Gluster Nodes):`

   `provisioner/gluster`

5. Enable and start Node Agent

   `systemctl enable tendrl-node-agent`

   `systemctl start tendrl-node-agent`

6. Install node-monitoring

   `yum install tendrl-node-monitoring`

7. Configure node-monitoring

   `Open /etc/tendrl/node-monitoring/node-monitoring.conf.yaml`

   `Update →`

   `etcd_connection = <IP of etcd server>`

8. Disable Firewall

    `service firewalld stop`

    `systemctl disable firewalld`

    `iptables --flush`

9. Enable and start node-monitoring

   `systemctl enable tendrl-node-monitoring`

   `systemctl start tendrl-node-monitoring`

## SELinux Configuration

   Tendrl does not currently support running on SELinux enabled systems. In case there are problems
   running tendrl on such systems, please set SELinux to 'Permissive' mode.
