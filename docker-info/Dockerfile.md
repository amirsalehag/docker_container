# ENTRYPOINT command
* `ENTRYPOINT` specifies the command that when ever the container runs, the command is what its going to be executed.  
# CMD command
* `CMD` can be used along with `ENTRYPOINT`, for example:  
```
ENTRYPOINT [“nginx”]
CMD [“-g”, “daemon off;”]
```
The difference is that when ever we use a seperate command while running the container, it overwrites the `CMD` commands, for example:  
```
docker run nginx:latest -v
```
it runs the command like `nginx -v`.  
# ENV command
* We use `ENV` variables in Dockerfile to use it inside of the Dockerfile:  
```
ENV PHPVERSION 7
RUN apk add --update apache2 php${PHPVERSION}-apache2 php${PHPVERSION}
```
We can even write the variables like `PHPVERSION=7` which is better because unlike the previous method, we can write multiple variables on the same line:  
```
ENV PHPVERSION=7 NGINXVERSION=1.2.1 TAG=tar
```
