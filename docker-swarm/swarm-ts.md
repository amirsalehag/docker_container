# docker.service not staying up
* If docker stops and could not be started, and it might have a log like this:  
```
ERRO[2022-12-11T09:32:02.718328123+03:30] error reading the kernel parameter net.ipv4.vs.conn_reuse_mode  error="open /proc/sys/net/ipv4/vs/conn_reuse_mode: no such file or directory"
ERRO[2022-12-11T09:32:02.718413621+03:30] error reading the kernel parameter net.ipv4.vs.expire_nodest_conn  error="open /proc/sys/net/ipv4/vs/expire_nodest_conn: no such file or directory"
ERRO[2022-12-11T09:32:02.718469781+03:30] error reading the kernel parameter net.ipv4.vs.expire_quiescent_template  error="open /proc/sys/net/ipv4/vs/expire_quiescent_template: no such file or directory"

PANI[2022-12-11T09:32:07.391253236+03:30] tocommit(273821) is out of range [lastIndex(225421)]. Was the raft log corrupted, truncated, or lost?  module=raft node.id=png0k9l1ezc3jg71itoigqtqx
```
the first three error logs can be solve by this command:  
```
sudo modprobe ip_vs
```
this command loads the kernel module for `ip_vs` networking.  
* And for the raft log we need to demote this node from another manager node:
```
docker node demote <the stopped node name>
```
and after that we need to remove it from the swarm cluster:  
```
docker node rm <the stopped node name>
```
and then we go back on the corrupted node and start the docker.service:  
```
sudo systemctl start docker.service
```
and then we can add it again, as manager or worker in the existing cluster.  

---
# losing the quorum
* If you lose the quorum of managers, you cannot administer the swarm. If you have lost the quorum and you attempt to perform any management operation on the swarm, an error occurs:  
```
Error response from daemon: rpc error: code = 4 desc = context deadline exceeded
```
The best way to recover from losing the quorum is to bring the failed nodes back online. If you can’t do that, the only way to recover from this state is to use the `--force-new-cluster` action from a manager node. This removes all managers except the manager the command was run from. The quorum is achieved because there is now only one manager. Promote nodes to be managers until you have the desired number of managers.  
From the node to recover, run:  
```
docker swarm init --force-new-cluster --advertise-addr node01:2377
```
