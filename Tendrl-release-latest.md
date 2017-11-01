This wiki page describes installation of Tendrl, Software Defined Storage Controller.

From Tendrl's point of view, there are these server roles:

* **Tendrl Server**: single machine which runs Tendrl itself (eg. Tendrl web ui and api runs there)
* **Tendrl Storage Node** aka **Storage Server**: machine on which  Software Defined Storage server (such as GlusterFS) is installed. There are multiple such machines, together forming a storage cluster.

Each role has a dedicated section with Tendrl installation steps specific for the role, but first there are few sections with
information not specific for any particular role.

When you already have a storage cluster installed (eg. GlusterFS Trusted Storage Pool hosting multiple Gluster volumes), you need one additional machine for Tendrl Server.

## Tendrl Server System Requirements

The server hosting tendrl-api/central_store should have minimum 12 GB of memory and 4 VCPUs (or equivalent)(due to alerts, logs being stored on this node)

See also tendrl-ansible `prechecks.yml` playbook file.

## Supported SDS

* Tendrl requires **Gluster>=3.12.0**

## Installation using Tendrl Ansible

You can perform installation of both Tendrl Server and Tendrl Storage Node
machines either manually (step by step following installation sections below)
or using tendrl-ansible. Using tendrl-ansible is highly recommended.

While tendrl-ansible automates the installation almost entirely, you still
need to roughly understand what steps are performed during installation of each
machine role, especially wrt configuration you may want to tweak.

Tendrl Ansible gives you option to change default configuration via ansible
variables. Description of all variables is provided in README file of each
ansible role.

To install tendrl-ansible, it's highly recommended to use rpm package provided
in the tendrl release repository:

```
# yum copr enable tendrl/release
# yum install tendrl-ansible
```

Quick introduction is provided in the README file provided with the package:

```
# less /usr/share/doc/tendrl-ansible-1.5.3/README.md
```

That said, you can also consult the release branch of tendrl-ansbile
repository:

https://github.com/Tendrl/tendrl-ansible/tree/release/1.5.3/


## SELinux Configuration

Tendrl provides [independent SELinux policy](https://fedoraproject.org/wiki/SELinux/IndependentPolicy), which is integral part of Tendrl.

To install the Tendrl SELinux policies, you need to switch SELinux mode to `permissive` on all Tendrl machines first: set `SELINUX=permissive` in `/etc/selinux/config` and then either run `setenforce 0` or reboot. Then you can install packages with Tendrl SELinux policies as described below.

On **Tendrl Server**:

* yum install carbon-selinux
* yum install tendrl-grafana-selinux
* yum install tendrl-selinux

On **Tendrl Storage Nodes**:

*  yum install tendrl-collectd-selinux
*  yum install tendrl-selinux

Warning: **running Tendrl on machines in enforcing mode doesn't work yet**, as Tendrl SELinux policies are in early stage of development. See [current list of known tendrl-selinux issues](https://github.com/Tendrl/tendrl-selinux/issues). Only when we gain more confidence in Tendrl SELinux polices based on fixing known issues and our testing, we will suggest to run Tendrl on machines in enforcing mode instead.

If you want to help with improvement of SELinux policies for Tendrl, [create issue for tendrl-selinux](https://github.com/Tendrl/tendrl-selinux/issues/new) and attach output of `ausearch -m avc` command along with your use case, which causes the avc denials.

SELinux configuration is covered in tednrl-ansible. By default all
machines are switched to permissive mode and listed packages are installed.

## Firewall Configuration

Tendrl does not currently support running on firewall enabled system as the
firewall rules are under development. Hence it is recommended to disable the
firewalld on server/storage nodes
 
```
service firewalld stop
systemctl disable firewalld
iptables --flush
```

Firewall configuration is covered in tednrl-ansible via
`workaround.disable-firewall.yml` playbook, which is included in
`site.yml.sample` example playbook.

## NTP

Make sure you keep time synchronized on all storage machines and Tendrl server.
When you install Tendrl on machines with already existing storage cluster, an ntp daemon (such as chrony or ntpd) is usually already configured because it's part of the storage cluster installation.

NTP configuration is out of scope of tendrl-ansible. Playbook `prechecks.yml`,
which is included in `site.yml.sample` playbook, only checks if the time
synchronization is configured.

## Https Configuration

Please refer to  https://github.com/Tendrl/documentation/wiki/Enabling-Https-on-tendrl-server

Please note that [there are known
issues](https://github.com/Tendrl/api/issues/303) and that https configuration
is not actively tested right now.

Configuration of https is not yet part of tendrl-ansible.

## Tendrl Server Installation

Installation steps listed there are covered in the following roles of
tendrl-ansible:

* grafana-repo
* tendrl-copr
* tendrl-server

The following procedure outlines the procedure to install tendrl server components manually:

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

   * `ETCD_LISTEN_CLIENT_URLS="http://<ip address of etcd server>:2379"`
   * `ETCD_ADVERTISE_CLIENT_URLS="http://<ip address of etcd server>:2379"`

   As a value for *etcd server ip address*, use some *public ip address of the
   tendrl server machine* (which is the server you are installing etcd on right
   now). This options controls where etcd server will listen on for client
   traffic.

   For more details, see [etcd configuration
   documentation](https://coreos.com/etcd/docs/3.2.5/op-guide/configuration.html).

   **To run secure ETCD (SSL/TLS based client server encryption and auth),
   please refer to:
   https://github.com/Tendrl/documentation/wiki/Tendrl-with-a-secure-etcd-cluster**
   Note: this is covered by tendrl-ansible, but it's disabled by default, as
   the issuing and deployment of tls certificates on all machines is out of
   scope of tendrl-ansible and you need to do it yourself first.

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
    etcd_connection: <FQDN of etcd server>
    graphite_host: <FQDN of Graphite Server>
    ```

    Note that:

    * when we use dns query to translate FQDN of etcd server to an ip address,
      the resulting value should match *ip address of etcd server* we
      configured just few steps ago
    * a safe default value for *FQDN address of graphite* would be a domain
      name which translates to ip address we
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
        :host: '<FQDN of etcd server>'
        :port: 2379
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
    CONF_DIR=/etc/tendrl/monitoring-integration/grafana/
    CONF_FILE=/etc/tendrl/monitoring-integration/grafana/grafana.ini
    ```

16. Create new strong password and set is as a value of `admin_password`
    option in `/etc/tendrl/monitoring-integration/grafana/grafana.ini` file.

    This password is used by Tendrl for internal purposes only.

    When one uses tendrl-ansible, this password is generated by [ansible
    password lookup
    plugin](https://docs.ansible.com/ansible/latest/playbooks_lookups.html#the-password-lookup)
    and stored in `grafana_admin_passwd` file.

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

18. Recall the password you created and added into `grafana.ini` config file
    few steps back, locate *Grafana credentials* section in
    `/etc/tendrl/monitoring-integration/monitoring-integration.conf.yaml`
    config file and set the same password there:

    ```
    # Grafana credentials
    credentials:
      user: admin
      password: set_the_same_password_as_used_for_grafana_admin_password
    ```

19. Enable and start monitoring-integration

    ```
    systemctl enable tendrl-monitoring-integration
    systemctl start tendrl-monitoring-integration
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

    When using tendrl-ansible, you create this `snmp.conf.yaml` file locally
    and set it's local path as a value of `tendrl_notifier_snmp_conf_file`
    ansible variable. See readme file of tendrl-server role for details.

24. Enable and start notifier service::

    ```
    systemctl enable tendrl-notifier
    systemctl start tendrl-notifier

    ```

25. Enable and start httpd

    ```
    systemctl enable httpd
    systemctl start httpd
    ```

26. Open the following URL in the browser

    ```
    http://<FQDN of the server>
    ```


## Tendrl Storage Node Installation

Installation steps listed there are covered in the following roles of
tendrl-ansible:

* tendrl-copr
* tendrl-storage-node

The following procedure outlines the procedure to install tendrl storage node components manually:

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
