## Server Installation

The following procedure outlines the procedure to install tendrl server components

1. Install CentOS 7.3

2. Enable the following repositories

   `wget https://copr.fedorainfracloud.org/coprs/tendrl/tendrl/repo/epel-7/tendrl-tendrl-epel-7.repo`

    `cp tendrl-tendrl-epel-7.repo /etc/yum.repos.d`

    `yum install epel-release`

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

9. Install tendrl dashboard

   `yum install tendrl-dashboard`

10. Restart httpd
   
    `systemctl restart httpd`

11. Firewall configurations

    `firewall-cmd --permanent --zone=public --add-port=2379/tcp`

    `firewall-cmd --permanent --zone=public --add-port=9292/tcp`

    `firewall-cmd --permanent --zone=public --add-port=80/tcp`

    `firewall-cmd --reload`

12. Open the following URL in the browser

    `http://<IP of the server>`

## Storage Node Installation

1. Install CentOS 7.3

2. Enable the following repositories

   `wget https://copr.fedorainfracloud.org/coprs/tendrl/tendrl/repo/epel-7/tendrl-tendrl-epel-7.repo`

    `cp tendrl-tendrl-epel-7.repo /etc/yum.repos.d`

    `yum install epel-release`

3. Install Node Agent

   `yum install tendrl-node-agent`

4. Configure Node Agent

   Edit the below configurations for connecting to the etcd server

   `Open /etc/tendrl/node-agent/node-agent.conf.yaml`

   `Update →`

   `etcd_connection = <IP of etcd server>`

5. Enable and start Node Agent

   `systemctl enable tendrl-node-agent`

   `systemctl start tendrl-node-agent`

6. Now the node gets registered to etcd server and the same will be listed in the UI

## SELinux Configuration

   Tendrl does not currently support running on SELinux enabled systems. In case there are problems
   running tendrl on such systems, please turn SELinux off.

   