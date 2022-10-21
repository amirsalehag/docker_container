# Docker-slim

In addition to reducing image sizes using multi-stage build an open source project with the name `docker-slim` exists that can drastically reduce image sizes even further.
There is no need to change the Dockerfile in any way — it works off the image that was created by the docker build command, analysing the built image to strip out everything that is not needed to run the image.  
Installing `docker-slim` and running the `docker-slim build` command on the control hub frontend image for demonstration purposes, I reduced the size to almost three times the original!  
The command that was run and its output follows:
```
docker-slim build \
--target nginx:1.21.1
--tag nginx:1.21.1-slim
```
---
