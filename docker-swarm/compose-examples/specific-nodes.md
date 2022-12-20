* When we want to deploy services on specific nodes, and with the replicated amount that we want it, we can use a compose file like this:  
```
version: "3"
services:
  nginx:
    image: nginx:latest
    ports:
      - 80:80
    deploy:
      mode: global
      restart_policy:
        condition: on-failure
      placement:
        constraints:
          - "node.labels.<added label>==<its value>"
          - "node.role==worker"
```
This means that we set the replication mode to global meaning that each of specified node only is going to have one nginx service, and the constraints means that it should be a worker and it also has to have the `elasticsearch=true` label specified on the worker nodes.  
* We can add labels to the nodes by this command:  
```
docker node update <node hostname on cluster> --label-add elasticsearch=true
```
and then its going to change and notify the managers immediately.  

---
