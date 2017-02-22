# Specific to issues (failure/errors/incomplete) observed during import

The following information would be required from each node in the cluster
* output of `ps aufx`
* output of `netstat -ntlp`
* output of `rpm -qa | grep tendrl`
* the contents of the `/etc/hosts file`
* output of `hostname`
* output of `ip addr ls`

Additionally, the output of the following from the python 2.x console:

`import socket`
`socket.getfqdn()`
`socket.gethostname()`
`socket.gethostbyaddr([ip])` # where [ip] is the IP address, run this once for each IP of the system

If the import issues are observed on a Gluster cluster, we would also need 
* output of `gluster peer list`
