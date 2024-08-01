# Multistage Docker Build

[Multistage Docker Build Beginner's Guide](https://medium.com/@younusraza909/docker-multi-stage-build-begginers-guide-a57ae6e49268)

## Benefits of Multistage Builds
- A multistage build allows you to use multiple images to build a final product.
- Single Dockerfile but can define multiple images and stages inside the same Dockerfile.
- Smaller Image Size
- Faster Deployment
- Enhanced Security

## Example: Angular Application

```dockerfile
# Step 1: Build the app in image 'builder'
FROM node:12.8-alpine AS builder
# Set the working directory
WORKDIR /usr/src/app
# Add the source code to app
COPY . .
# Generate the build of the application
RUN yarn && yarn build

# Step 2: Use build output from 'builder'
FROM nginx:stable-alpine
# Use Labels for organizing Docker Images
LABEL version="1.0"
# Add the nginx.conf file 
COPY nginx.conf /etc/nginx/nginx.conf
# Set the working directory
WORKDIR /usr/share/nginx/html
# Copy the build output to replace the default nginx contents
COPY --from=builder /usr/src/app/dist/my-angular-app/ .
```

> **Note**: Normally, Stage 1 initializes the first build, and then Stage 2 initializes a secondary build stage like that.


### Key Points:
1. **Final Image Stage**: Make sure to define the final image that will be run. In the example, `nginx:stable-alpine` is used.
2. **Labeling**: Use `LABEL` to add metadata to the image.
3. **Configuration**: Copy necessary configuration files using `COPY`.
4. **Expose Ports**: Use `EXPOSE` to specify the ports on which the container listens.
5. **Entrypoint or CMD**: Use `CMD` to define the command that will run when the container starts.

This structure ensures that your Dockerfile ends correctly, preparing the final image for deployment.
