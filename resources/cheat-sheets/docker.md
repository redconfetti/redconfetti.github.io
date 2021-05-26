---
layout: page
title: Docker
---
[Back to Cheat Sheets](/resources/cheat-sheets/)

## Docker

``` shell
# View docker client and server version info
docker version

# List Running Containers
docker ps

# List All Containers
docker ps -a

# Initialize and start an 'nginx' container with "web" as name
docker run -d -P --name web nginx

# Initialize a container running in the background with a local directory mounted in the container, based on the nginx container
docker run -d -P -v $HOME/site:/usr/share/nginx/html --name mysite nginx

# View container ports
docker port [container_id]

# Start container
docker start [container_id]

# Restart container
docker restart [container_id]

# Stop Container
docker stop [container_id]

# SSH into running Docker container for inspection / debugging
docker attach [container_id]

# Remove Docker container (requires stop to remove)
docker stop [container_id]
docker rm [container_id]

# Display stdout from container
docker logs [container_id]

# Display processes for container
docker top [container_id]

# Copy files/folders from the local filesystem into a container
docker cp foo.txt mycontainer:/foo.txt
docker cp /Projects/myapp mycontainer:/app

# Copy files/folders from the contain to the local filesystem
docker cp [container_name]:/foo.txt foo.txt

# Get JSON data on container
docker inspect [container_id]

# View local docker images
docker images

# Remove docker image
docker rmi [image_id]

# Search for Docker images
docker search [image_name]

# Download Docker image for later use
docker pull [image_name]

# Build Docker image from Git repository (requires Dockerfile)
docker build https://github.com/username/reponame.git#master

# Tag Docker image in preparation for push to remote repository
docker tag [image_id] [repository_url]:[port]/[name]:[tag]

# Push Docker image to remote repository
docker push [repository_url]:[port]/[name]:[tag]
```
