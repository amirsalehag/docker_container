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
