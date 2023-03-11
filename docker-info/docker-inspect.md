# containers health checks
* output only the url of health check:  
```
docker inspect <container name> --format "{{ index .Config.Healthcheck.Test }}" | sed 's/^.\(.*\).$/\1/' |  grep -o 'http[^ ]*'
```
* output all the health check command:  
```
docker inspect <container name> --format "{{ index .Config.Healthcheck.Test }}" | sed 's/^.\(.*\).$/\1/' | sed "s/^[^ ]* //"
```

---
