###################### Multistage docker build ###############################

https://medium.com/@younusraza909/docker-multi-stage-build-begginers-guide-a57ae6e49268


-A multistage build allows you to use multiple images to build a final product.
- single Dockerfile but can define multiple images and stages inside same Dockerfile.
- Smaller Image Size
- Faster Deployment
- Enhanced Security

e.g angular application 
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
# Use Labels for organize Docker Images
LABEL version="1.0"
# Add the nginx.conf file 
COPY nginx.conf /etc/nginx/nginx.conf
# Set the working directory
WORKDIR /usr/share/nginx/html
# Copy the build output to replace the default nginx contents
COPY --from=builder /usr/src/app/dist/my-angular-app/ .

Note- noramally stage:1 initialize a first build and then Stage 2: Initializes a secondary build stage like that.



