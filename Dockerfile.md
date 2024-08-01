# let’s start building Dockerfile from scratch 

Pre-Requisite: Docker should be installed on your system

- **FROM**: The FROM instruction specifies the base image that the container will be built on top of. This instruction is typically the first one in a Dockerfile and is used to set the base image for the container. 
   
              `FROM <image>`

  For example, 
              `FROM node:14-alpine3.16`
   
  This instruction tells Docker to use the node:14-alpine3.16 image as the base image for the container. To use a specific version or tag of an image you can use:<version> or:<tag> syntax.

- **WORKDIR**: WORKDIR instruction sets the working directory for any command that follows it in the Dockerfile.
  This means that any commands that are run in the container will be executed relative to the specified directory. 

             `WORKDIR <directory>`

  For example,
             `WORKDIR /app`

  This instruction tells Docker to set the working directory of the container to /app. Any subsequent commands in the Dockerfile, such as COPY, RUN, or CMD, will be executed in this directory.

  **Note**: It’s important to note that the WORKDIR instruction creates the directory if it does not already exist. And the subsequent COPY and ADD commands will be executed relative to the WORKDIR specified.

- **COPY**: COPY instruction to copy local files from the host machine to the current working directory. 
  For example, to copy a file named package.json from the host machine’s current directory to the image’s /app directory, you would use the following command:

            `COPY package.json /app/`

  If you want to copy all the files from the host’s current directory to the container’s current directory, you can use the below command:
  
            `COPY . .`

- **RUN**: RUN instruction to execute commands that will run during the image build process. 

            `RUN <command_name>`
  For example, to update the package manager and install a specific package, you would use the following command:
    
            `RUN npm install`

- **CMD**: CMD instruction sets the command that will be executed when a container is run from the image.
           Perform default actions in container
           CMD is like a default action that you can change if you want.

   The format of the instruction is:
       
           `CMD ["executable","param1","param2",...]`
	
  For example,
           `CMD ["npm", "start"]`
  
- **ENTRYPOINT** : The ENTRYPOINT Dockerfile instruction sets the process that’s executed when your container starts.
                   Entrypoint use for specifc actions in container.
                   ENTRYPOINT is like given action that the robot must follow no matter what.

  For example,
           `ENTRYPOINT ["python", "app.py"]`
	
  Apart from all the instructions mentioned, there are some more instructions like ENV, and EXPOSE which are also used in creating Dockerfile. Below is a detailed description of the same:

- **ENV**: ENV instruction to set environment variables inside the image which will be available during build time as well as in a running container. For example, to set the NODE_ENV environment variable to production, you would use the following command:

    `ENV NODE_ENV production`

- **EXPOSE**: EXPOSE command to tell Docker which ports the container will listen on at runtime. For example, if your application listens on port 9000, you would use the following command:
    
    `EXPOSE 9000`

- Sample Docker File would look like below:

```dockerfile
FROM node:14-alpine3.16

WORKDIR /app

COPY . .

RUN npm install

CMD [ "npm", "start" ]
```
