# What is docker swarm
Swarm provides a platform for deploying and managing a containerized application across multiple docker hosts.  
It automates the process of deploying new services or changes into an existing service.changes may include  
anything declared inside the service definition (docker-compose.yml).  

---
# Starting swarm cluster
* When we join a docker engine host to a swarm cluster, we specify whether it should be a manager or worker.  
* Most production swarm deployments should have three or five manager nodes, more numbers equal more availability  
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
I recommend you be specific and always use both flags. the default port that swarm mode operates on is 2377. this is customizable, but it’s  
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
* We can use 2 ways of bringing up the services on swarm:  
1. use `docker service` command.  
2. 
* We used `docker service create` to tell Docker we are declaring a new service:  
```
docker service create --name web-fe -p 8080:8080 --replicas 5 nginx:latest
```
the `--name` flag to name it `web-fe`.we told Docker to map port 8080 on every node in the swarm to 8080 inside of each service replica.  
Next, we used the `--replicas` flag to tell Docker there should always be 5 replicas of this service. Finally,we told Docker which image  
to use for the replicas, it’s important to understand that all service replicas use the same image and configs.  
* use the `docker service ls` command to see a list of all services running on a swarm:  
```
$ docker service ls
ID           NAME        MODE       REPLICAS        IMAGE             PORTS  
z7o...uw    web-fe    replicated      5/5        nginx:latest   *:8080->8080/tcp  
```
* Use the `docker service ps` command to see a list of service replicas and the state of each:  
```
   ID         NAME          IMAGE        NODE         DESIRED           CURRENT  
817...f6z   web-fe.1      nginx/...      mgr2         Running        Running 2 mins
a1d...mzn   web-fe.2      nginx/...      wrk1         Running        Running 2 mins
```
* For detailed information about a service, use this command:  
```
docker service inspect --pretty web-fe
```
the `--pretty` to limit the output to the most interesting items.  
* The other replication mode is global, which runs a single replica on every node in the swarm.  
To deploy a global service you need to pass the `--mode global` flag to the `docker service create` command.
*
