# docker stack
When using swarm mode for containers orchestration, we can deploy our application services with two methods,  
first using the `docker service` command, second using the `docker stack` command.  
The first method is like running the `docker container run` command which means everything should be specified as the one line commands,  
but the second way is like running `docker compose` command which means that we should have a `docker-compose.yml` file.  

---
# docker-compose file for stack
You can read the compose file [document](https://docs.docker.com/compose/compose-file/compose-file-v3/) which is also used for deploying the swarms stack.  

---
## deploy stack
* You can deploy a stack using a compose file with this command:  
```
docker stack deploy -c <compose file path> <stack name>
```
* We can update a stack after some changes for example to the compose file with the same command above.
---
