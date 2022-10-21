# Removing any dangling images and stoped containers
When creating containers, we can specify `--rm` as an argument to `docker run` to delete the container after its been stoped.  
Watch this [video](https://www.youtube.com/watch?v=0vxIyXgkihA) for more details.

---
# Saving images and containers
* For saving images as an archive file, we use `docker save` command. use this command for archiving and compressing at the same time:  
```
docker save (image name) | gzip > (file name).tar.gz
```
* For saving a running container with all its changes as an archive file we use `docker export` command. use `docker export` instead  
of `docker save` in the command above to do the same thing.  

---
