###################### Bind mount and volume mount and Tmpfs mounts #########################


https://medium.com/dev-sec-ops/docker-101-volume-bind-mounting-8f200c14ca0
https://medium.com/@younusraza909/persisting-data-in-docker-volumes-bind-mount-5c0402b0f731

1) Bind mount and volume mount 
- A volume mount is a great choice when you need somewhere persistent to store your application data. A bind mount is another type of mount, which lets you share a directory from the host's filesystem into the container. When working on an application, you can use a bind mount to mount source code into the container.

-The main difference between bind mounting and volume mounting is that we use bind mount to create the persistent volume on the host machine and not in the docker host directory whereas volume mounting does it on the docker host and hence you need to provide the entire directory name while mounting.

##Volume Mounting
- Docker Volume is a mechanism that helps us to store data outside the container. 
- volume mounting is used to prevent loss of data 
- Volume in docker host
- i.e. when the docker container is shut down due to whatever reason the data should not be lost and must be retained.
- All volumes are managed by Docker and stored in a dedicated directory on your host, usually  /var/lib/docker/volumes
- when we want to use a database and want to store data across containers.

#create the persistent volume 
$docker volume create data_volume

#PV link it with the docker container. use to mount the volume in-side the docker container.
$docker run -v data_volume:/var/lib/mysql mysql

# how can we define them while spinning up containers.
$docker run -it -v test_volume:/test --name ubuntu_container ubuntu:22.04

#We can make a volume read-only 
$docker run -it -v test_name_volume:/test:ro ubuntu:22.04
Note - The trick is here that even if we just type in the command just above us without typing the volume create command, docker would itself create the data_volume automatically

#Defining Volumes In Dockerfile

FROM ubuntu:22.04
VOLUME /test_name_volume

#Defining Volumes In Compose File

services:
  app:
    image: ubuntu:22.04
    volumes:
      - test_name_volume:/data
volumes:
  test_name_volume:

#If we want to tell compose that volume is already created and just pick that volume and mount add this option into volume section.

volumes:
  test_name_volume:
    external: true
	
# Inspecting A Volume
$ docker volume ls
$docker volume inspect test_name_volume

#Removing A Volume
$ docker volume rm test_name_volume
test_name_volume	

##Bind Mounting
- Bind Mount has been made available since the early stage of Docker. Bind mounts will mount a file or folder from your host machine to the container file system. When we start a container that file or folder does not necessarily exist in the container. If it doesn’t exist, it will be created on demand. Now when we change something on that folder (that is mounted) it will be reflected inside the container and vice versa.

Note - Now it might introduce some security risks that you need to handle because if we change something from the container it will be reflected in the host file system as well.
We usually use mounts when we are developing. During development, we mount our code folder into the container.
Then, when we make changes to the code, it will reflect in the container in real time without us needing to create a new image.

#This is the command to link the new docker container with the persistent volume that we already have in our local system when we set up in the previous step of configuring our volume mounting.
$ docker run -v /data/mysql:/var/lib/mysql mysql

Note - -v is the old way of doing things, the new methodology is to use the --mount command
$ docker run \ --mount type=bind, source=/data/mysql, target=/var/lib/mysql  mysql

## Tmpfs mounts 
-Mount is a special type of mount that allows you to create a temporary file system in memory
-tmpfs mount is temporary and exists only as long as the container is running.
-Once the container is stopped or restarted, the data in the tmpfs mount is lost.
-Therefore, tmpfs mounts are not suitable for storing data that needs to persist across container restarts.

#When to use Tmpfs Mounts:
- As Temporary Scratch Space: If your application requires temporary storage for processing data, such as caching or intermediate calculations

#How to use Tmpfs Mount
$docker run --tmpfs /path/in/container image:tag

#Tmpfs Mount compose
services: 
myservice: 
  image: image:tag 
  tmpfs: - /path/in/container
