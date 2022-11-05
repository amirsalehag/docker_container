# ENTRYPOINT command
* `ENTRYPOINT` specifies the command that when ever the container runs, the command is what its going to be executed.  
# CMD command
* `CMD` can we used along with `ENTRYPOINT`, for example:  
```
ENTRYPOINT [“nginx”]
CMD [“-g”, “daemon off;”]
```
the difference is that when ever we use a seperate command while running the container, it overwrites the `CMD` commands, for example:  
```
docker run nginx:latest -v
```
it runs the command like `nginx -v`.  
