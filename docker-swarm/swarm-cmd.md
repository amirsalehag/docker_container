# What is docker swarm
Swarm provides a platform for deploying and managing a containerized application across multiple docker hosts.  
It automates the process of deploying new services or changes into an existing service.changes may include  
anything declared inside the service definition (docker-compose.yml).  

---
* When we join a docker engine host to a swarm cluster, we specify whether it should be a manager or worker.  
* Most production swarm deployments should three or five manager nodes, more numbers equal more availability  
but it takes more time for managers to acknowledge changes to cluster.  
* These ports should be open for swarm to function:  
TCP port 2377 for cluster management communications.  
TCP/UDP port 7946 for communication among nodes.  
UDP port 4789 for overlay network traffic.  
