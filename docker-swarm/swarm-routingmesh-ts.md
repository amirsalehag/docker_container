# routing mesh not working
when we have different nodes on an esxi vmware, there could be a few reason that might have caused that.  
one could be that the UDP packets are being dropped by the source node. Disabling checksum offloading appears to resolve this issue.  
Run the following on all the VMs in your cluster:  
```
ethtool -K [network] tx-checksum-ip-generic off
```
the network could be the anything that the docker swarm is using to communicate with others.  
