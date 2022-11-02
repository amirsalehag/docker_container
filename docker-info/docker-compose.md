# HEALTH-CHECK
* The `HEALTHCHECK` instruction tells Docker how to test a container to check that it is still working.  

The options that can appear before CMD are:  
* --interval=DURATION (default: 30s)  
* --timeout=DURATION (default: 30s)  
* --start-period=DURATION (default: 0s)  
* --retries=N (default: 3)  

The health check will first run interval seconds after the container is started, and then again interval seconds after each previous check completes.
If a single run of the check takes longer than timeout seconds then the check is considered to have failed.  
It takes retries consecutive failures of the health check for the container to be considered unhealthy.  
* The command’s exit status indicates the health status of the container. The possible values are:  
#### 0: success - the container is healthy and ready for use  
#### 1: unhealthy - the container is not working correctly  
#### 2: reserved - do not use this exit code  
For example, to check every five minutes or so that a web-server is able to serve the site’s main page within three seconds:
```
HEALTHCHECK --interval=5m --timeout=3s CMD curl -f http://localhost/ || exit 1
```
