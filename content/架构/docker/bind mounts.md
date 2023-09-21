
"Bind mounts" is one way to store docker container data. A file or directory on the host machine is mounted into a container when you use a bind mount. Like [[volumes]], you can use both `-v` and `--mount` flags to mount the directory.

```shell
docker run -d -v "/var/data/target:/app" nginx:latest
# or
docker run -d --mount type=bind,source=/var/data/target,target=/app nginx:latest
```