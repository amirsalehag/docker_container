# ENTRYPOINT command
* `ENTRYPOINT` specifies the command that when ever the container runs, the command is what its going to be executed.  
# CMD command
* `CMD` can be used along with `ENTRYPOINT`, for example:  
```dockerfile
ENTRYPOINT [“nginx”]
CMD [“-g”, “daemon off;”]
```
The difference is that when ever we use a seperate command while running the container, it overwrites the `CMD` commands, for example:  
```dockerfile
docker run nginx:latest -v
```
it runs the command like `nginx -v`.  
# ENV command
* We use `ENV` variables in Dockerfile to use it inside of the Dockerfile:  
```dockerfile
ENV PHPVERSION 7
RUN apk add --update apache2 php${PHPVERSION}-apache2 php${PHPVERSION}
```
We can even write the variables like `PHPVERSION=7` which is better because unlike the previous method, we can write multiple variables on the same line:  
```dockerfile
ENV PHPVERSION=7 NGINXVERSION=1.2.1 TAG=tar
```
---
# Here-Documents
* We can use here-documents `EOF` in our dockerfile to path multiple dockerfile instruction lines to `RUN` or `COPY` commands.

Example:
* Running a multi-line script:
```dockerfile
FROM debian
RUN <<EOT
mkdir -p foo/bar
EOT
```
or
```dockerfile
FROM debian
RUN <<FILE1 cat > file1 && <<FILE2 cat > file2
I am
first
FILE1
I am
second
FILE2
```
[link](https://docs.docker.com/engine/reference/builder/#here-documents) to here-doc dockerfile documentation.

