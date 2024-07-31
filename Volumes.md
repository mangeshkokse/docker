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
