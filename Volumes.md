# Docker Mounts: Bind Mounts, Volume Mounts, and Tmpfs Mounts

## References
- [Docker 101: Volume & Bind Mounting](https://medium.com/dev-sec-ops/docker-101-volume-bind-mounting-8f200c14ca0)
- [Persisting Data in Docker: Volumes & Bind Mount](https://medium.com/@younusraza909/persisting-data-in-docker-volumes-bind-mount-5c0402b0f731)

## Bind Mount and Volume Mount

- A **volume mount** is a great choice when you need somewhere persistent to store your application data. A **bind mount** is another type of mount, which lets you share a directory from the host's filesystem into the container. When working on an application, you can use a bind mount to mount source code into the container.

- The main difference between bind mounting and volume mounting is that:
  - **Bind mount**: Creates the persistent volume on the host machine and not in the Docker host directory.
  - **Volume mount**: Creates the volume in the Docker host directory, so you need to provide the entire directory name while mounting.

## Volume Mounting

- Docker Volume is a mechanism that helps us to store data outside the container.
- Volume mounting is used to prevent loss of data.
- Volumes are managed by Docker and stored in a dedicated directory on your host, usually `/var/lib/docker/volumes`.
- Useful when you want to use a database and store data across containers.

### Commands

```bash
# Create the persistent volume
docker volume create data_volume

# Link the volume with the Docker container. Use to mount the volume inside the Docker container.
docker run -v data_volume:/var/lib/mysql mysql

# Define volumes while spinning up containers
docker run -it -v test_volume:/test --name ubuntu_container ubuntu:22.04

# Make a volume read-only
docker run -it -v test_name_volume:/test:ro ubuntu:22.04
```
> **Note**: The trick is that even if you use the command below without explicitly creating the volume, Docker will automatically create the volume if it does not exist.

## Defining Volumes in Dockerfile

```dockerfile
FROM ubuntu:22.04
VOLUME /test_name_volume
```
## Defining Volumes in Docker Compose File
```yaml
services:
  app:
    image: ubuntu:22.04
    volumes:
      - test_name_volume:/data

volumes:
  test_name_volume:
```
## If You Want to Use an Existing Volume
- To tell Compose that a volume is already created and just pick that volume to mount, add this option to the volume section:
```yaml
volumes:
  test_name_volume:
    external: true
```
### Inspecting and Removing Volumes
```bash
# Inspect a volume
docker volume ls
docker volume inspect test_name_volume

# Remove a volume
docker volume rm test_name_volume
```
## Bind Mounting

Bind Mount has been available since the early stage of Docker. Bind mounts will mount a file or folder from your host machine to the container file system. When we start a container, that file or folder does not necessarily exist in the container. If it doesn’t exist, it will be created on demand. Now, when we change something in that folder (that is mounted), it will be reflected inside the container and vice versa.

> **Note**: Bind mounts might introduce some security risks that you need to handle because if we change something from the container, it will be reflected in the host file system as well. We usually use mounts during development. During development, we mount our code folder into the container. Then, when we make changes to the code, it will reflect in the container in real-time without us needing to create a new image.

### Linking Docker Container with Persistent Volume

This is the command to link a new Docker container with the persistent volume that we already have in our local system when we set up in the previous step of configuring our volume mounting:

```sh
$ docker run -v /data/mysql:/var/lib/mysql mysql
```

**Note**:-v is the old way of doing things. The new methodology is to use the --mount command
```sh
$ docker run --mount type=bind,source=/data/mysql,target=/var/lib/mysql mysql
```
## Tmpfs Mounts

Tmpfs mount is a special type of mount that allows you to create a temporary file system in memory. A tmpfs mount is temporary and exists only as long as the container is running. Once the container is stopped or restarted, the data in the tmpfs mount is lost. Therefore, tmpfs mounts are not suitable for storing data that needs to persist across container restarts.

### When to use Tmpfs Mounts:
- **As Temporary Scratch Space**: If your application requires temporary storage for processing data, such as caching or intermediate calculations.

### How to use Tmpfs Mount
```sh
$ docker run --tmpfs /path/in/container image:tag
```
### Tmpfs Mount in Docker Compose
```yaml
services:
  myservice:
    image: image:tag
    tmpfs:
      - /path/in/container
```




