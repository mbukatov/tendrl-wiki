# Multinode Glusterfs with All-in-1 Tendrl for Monitoring Vagrant - Intern Edition
#
# Author: Nathan Weinberg
# Date: 5 June 2018
#
#### Based heavily off README by julenlim found [here](https://github.com/julienlim/multinode-glusterfs-with-tendrl-vagrant)


## Initial Setup

### Step 1
Install VirtualBox and Vagrant (rpm/yum)

You may have to run the command `$ sudo dnf install kernel-devel dkms kernel-headers`

### Step 2
Put Ju's "Vagrantfile" and "bootstrap.sh" in a new directory

### Step 3
Navigate to your new directory and run `$ vagrant up --provider virtualbox`

By this point you should have 4 VMs (node0...node3) up and running.

### Step 4
```bash
$ vagrant ssh node0
$ ssh-keygen
$ cat /root/.ssh/id_rsa.pub
```
Copy the contents of this file to your clipboard

### Step 5
On nodes 1-3, run `$ ssh-keygen`, then paste your clipbaord contents (SSH key from node0) into a new file located at /root/.ssh/authorized_keys

### Step 6
On *all* nodes, do the following:

- Edit /etc/ssh/sshd_config such that the following settings are configured
	- Permit root login yes
	- RSAAuthentication yes
	- Pubkey Authentication yes
	- PasswordAuth no

- Edit /etc/hosts such that each node has the ip address of the others follwed by the node name
	- ip addresses can be found `$ ip a`

- Run `$ service sshd restart`

By this point you should be able to passwordless SSH from node0 to all other nodes.

### Step 7
On nodes 1-3, do the following:

```bash
$ fdisk /dev/sdb
$ mkfs.xfs /dev/sdb1
$ parted /dev/sdb print
```

Then uncomment "# /dev/sdb1..." in /etc/fstab and `$ mount -a`

Verify with `$df -k`

### Step 8
SSH into node1 and run
```bash
$ gluster peer probe node2
$ gluster peer probe node3
$ gluster peer status

$ gluster volume create vol1 node1:/bricks/brick1 node2:/bricks/brick1 node3:/bricks/brick1 force
$ gluster volume start vol1
$ gstatus -a
```

By this point your gluster cluster should be up and running.

### Step 9
Exit back into node0 and run
```bash
$ cd /etc/yum.repos.d 
$ wget https://copr.fedorainfracloud.org/coprs/tendrl/release/repo/epel-7/tendrl-release-epel-7.repo
$ yum install tendrl-ansible
```

### Step 10

Run `$ cp /usr/share/doc/tendrl-ansible-VERSION/site.yml .` such that VERSION is your version of tendrl-ansible.

Then create a new file "inventory_file" that looks as follows (IP address may vary):

```text
[gluster_servers]
node1
node2
node3
[tendrl_server]
node0
[all:vars]
# Mandatory variables. In this example, 192.0.2.1 is ip address of tendrl
# server, tendrl.example.com is a hostname of tendrl server and
# tendrl.example.com hostname is translated to 192.0.2.1 ip address.
etcd_ip_address=172.28.128.3
etcd_fqdn=172.28.128.3
graphite_fqdn=172.28.128.3
configure_firewalld_for_tendrl=false
# when direct ssh login of root user is not allowed and you are connecting via
# non-root cloud-user account, which can leverage sudo to run any command as
# root without any password
#ansible_become=yes
#ansible_user=cloud-user
```

### Step 11
Run `$ ansible-playbook -i inventory_file site.yml`. If you run into issues try running `$ ansible -i inventory_file -m ping all` and ensure all nodes are able to communicate with one another.

You should now be able to access the Tendrl dashboard from you machine via a browser at this URL: `http://<node0-ip-address>/`