# What is docker swarm
Swarm provides a platform for deploying and managing a containerized application across multiple docker hosts.  
It automates the process of deploying new services or changes into an existing service.changes may include  
anything declared inside the service definition (docker-compose.yml).  

---
# Starting swarm cluster
* When we join a docker engine host to a swarm cluster, we specify whether it should be a manager or worker.  
* Most production swarm deployments should three or five manager nodes, more numbers equal more availability  
but it takes more time for managers to acknowledge changes to cluster.  
* These ports should be open for swarm to function:  
TCP port 2377 for cluster management communications.  
TCP/UDP port 7946 for communication among nodes.  
UDP port 4789 for overlay network traffic.  
* Also note, with encrypted overlay networks you will need to allow inbound traffic on the workers for the ESP protocol.
```
sudo ufw allow proto esp
```
---
* Initialize a new swarm:  
```
docker swarm init --advertise-addr <HOSTIPADDRESS> --listen-addr <HOSTIPADDRESS>:2377
```
`docker swarm init` this tells Docker to initialize a new swarm and make this node the first manager. It also enables swarm mode on the node.  
`--advertise-addr` As the name suggests, this is the swarm API endpoint that will be advertised to other nodes in the swarm.  
It will usually be one of the node’s IP addresses, but can be an external load-balancer address. It’s an optional flag unless you want to  
specify a load-balancer or specific IP address on a node with multiple interfaces.  
`--listen-addr` this is the IP address that the node will accept swarm traffic on. If not explicitly set, it defaults to the same value as  
`--advertise-addr` If --advertise-addr is a load-balancer, you must use --listen-addr to specify a local IP or interface for swarm traffic.  
I recommend you be specific and always use both flags. the default port that swarm mode operates on is 2377. is is customizable, but it’s  
convention to use 2377/tcp for secured (HTTPS) client-to-swarm connections.  
* Run the `docker swarm join-token` command from manager to extract the commands and tokens required to add new workers and  
managers to swarm:  
```
docker swarm join-token worker
```
```
docker swarm join-token manager 
```
(You should ensure that your join tokens are kept secure, as they’re the only thing required to join a node to a swarm.)  
* Log on to another server, and then use the workers generated token if you want to add the node as a worker:  
```
docker swarm join --token <WORKERJOINTOKEN> <MANAGERIPADDRESS> --advertise-addr <HOSTIPADDRESS>:2377 --listen-addr <HOSTIPADDRESS>:2377
```
and for it to be the manager node, we give to it the manager token istead.  
* List the nodes in the swarm by running docker node ls from any of the manager nodes in the swarm:
```
docker node ls
```
