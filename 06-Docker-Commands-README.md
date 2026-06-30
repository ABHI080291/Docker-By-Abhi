# 06 - Docker Commands Complete Reference 🔧

**Master all Docker commands with practical examples**

---

## 📚 Table of Contents

1. [Image Commands](#image-commands)
2. [Container Commands](#container-commands)
3. [Network Commands](#network-commands)
4. [Volume Commands](#volume-commands)
5. [System Commands](#system-commands)
6. [Registry Commands](#registry-commands)
7. [Build Commands](#build-commands)
8. [Utility Commands](#utility-commands)

---

## Image Commands

### Pull Images

```bash
# Pull latest version
$ docker pull ubuntu

# Pull specific version
$ docker pull python:3.9

# Pull from specific registry
$ docker pull gcr.io/project/image:tag

# Pull all tags
$ docker pull --all-tags python
```

### List Images

```bash
# List all images
$ docker images

# List images with size
$ docker images --format "table {{.Repository}}\t{{.Size}}"

# List specific images
$ docker images python

# List image IDs only
$ docker images -q

# Filter images
$ docker images -f "reference=python:*"
```

### Build Images

```bash
# Build from Dockerfile
$ docker build -t myimage:1.0 .

# Build with specific file
$ docker build -f Dockerfile.prod -t app:1.0 .

# Build with build arguments
$ docker build --build-arg VERSION=1.0 -t app:1.0 .

# Build without cache
$ docker build --no-cache -t app:1.0 .

# Build with labels
$ docker build -t app:1.0 --label version=1.0 .
```

### Tag Images

```bash
# Create tag/alias
$ docker tag myimage:1.0 myimage:latest

# Tag for registry
$ docker tag app:1.0 myregistry.com/app:1.0

# Tag multiple
$ docker tag app:1.0 app:1.0-alpine
$ docker tag app:1.0 app:latest
```

### Remove Images

```bash
# Remove image
$ docker rmi myimage:1.0

# Remove multiple
$ docker rmi image1 image2 image3

# Force remove
$ docker rmi -f myimage:1.0

# Remove dangling images
$ docker image prune

# Remove all unused
$ docker image prune -a
```

### Image Information

```bash
# Inspect image
$ docker inspect myimage:1.0

# View image history
$ docker history myimage:1.0

# View layers
$ docker history --no-trunc myimage:1.0

# Save image to file
$ docker save myimage:1.0 > myimage.tar

# Load from file
$ docker load < myimage.tar
```

### Search Images

```bash
# Search Docker Hub
$ docker search python

# Search official only
$ docker search --is-official python

# Limit results
$ docker search --limit 5 python
```

---

## Container Commands

### Run Containers

```bash
# Basic run
$ docker run ubuntu echo "Hello"

# Run in background
$ docker run -d nginx

# Run with name
$ docker run --name my-web nginx

# Run with port mapping
$ docker run -p 8080:80 nginx
$ docker run -p 8080:80 -p 8443:443 nginx

# Run with volume
$ docker run -v /host:/container nginx

# Run with environment
$ docker run -e KEY=value image

# Run interactive
$ docker run -it ubuntu bash

# Run with resource limits
$ docker run -m 512m image
$ docker run --cpus 1.5 image

# Run with restart policy
$ docker run --restart always image
$ docker run --restart on-failure:5 image

# Run with hostname
$ docker run --hostname myhost image

# Run on network
$ docker run --network mynetwork image

# Run as user
$ docker run --user 1000 image

# Run with working directory
$ docker run -w /app image

# Run with entrypoint override
$ docker run --entrypoint /bin/sh image
```

### List Containers

```bash
# List running
$ docker ps

# List all
$ docker ps -a

# List last created
$ docker ps -l

# List IDs only
$ docker ps -q

# List all IDs
$ docker ps -a -q

# Filter containers
$ docker ps -f "status=running"
$ docker ps -f "name=web"

# Custom format
$ docker ps --format "table {{.Names}}\t{{.Status}}"
```

### Manage Containers

```bash
# Start container
$ docker start container-id

# Stop container
$ docker stop container-id

# Restart container
$ docker restart container-id

# Kill container
$ docker kill container-id

# Pause container
$ docker pause container-id

# Unpause
$ docker unpause container-id

# Remove container
$ docker rm container-id

# Remove running container
$ docker rm -f container-id

# Remove multiple
$ docker rm container1 container2

# Remove all stopped
$ docker container prune
```

### Container Info

```bash
# Inspect container
$ docker inspect container-id

# View logs
$ docker logs container-id

# Follow logs
$ docker logs -f container-id

# Last N lines
$ docker logs --tail 50 container-id

# With timestamps
$ docker logs -t container-id

# Since specific time
$ docker logs --since 2024-01-15T10:00:00 container-id

# Statistics
$ docker stats container-id

# Running processes
$ docker top container-id

# Port mappings
$ docker port container-id

# Full details
$ docker inspect -f '{{json .}}' container-id
```

### Execute Commands

```bash
# Execute command
$ docker exec container-id command

# Interactive shell
$ docker exec -it container-id bash

# As root
$ docker exec -u root container-id command

# With environment
$ docker exec -e VAR=value container-id command

# Get exit code
$ docker exec container-id echo $?
```

### Copy Files

```bash
# Copy from container
$ docker cp container-id:/path/file ./

# Copy to container
$ docker cp ./file container-id:/path/

# Copy directory
$ docker cp ./dir container-id:/path/
```

### Container Lifecycle

```bash
# Create (don't start)
$ docker create image-name

# Start created container
$ docker start container-id

# Commit container to image
$ docker commit container-id myimage:1.0

# Export container
$ docker export container-id > container.tar

# Rename
$ docker rename old-name new-name
```

---

## Network Commands

### Create Networks

```bash
# Create bridge network
$ docker network create mynet

# Create overlay (Swarm)
$ docker network create -d overlay mynet

# Create with subnet
$ docker network create --subnet=172.20.0.0/16 mynet

# Create with gateway
$ docker network create --subnet=172.20.0.0/16 \
  --gateway=172.20.0.1 mynet
```

### List Networks

```bash
# List networks
$ docker network ls

# Inspect network
$ docker network inspect mynet

# Filter networks
$ docker network ls --filter "driver=bridge"
```

### Connect Containers

```bash
# Connect to network
$ docker network connect mynet container-id

# Disconnect from network
$ docker network disconnect mynet container-id

# Connect with alias
$ docker network connect --alias myalias mynet container-id
```

### Remove Networks

```bash
# Remove network
$ docker network rm mynet

# Remove multiple
$ docker network rm net1 net2

# Remove unused
$ docker network prune
```

---

## Volume Commands

### Create Volumes

```bash
# Create volume
$ docker volume create myvolume

# Create with driver
$ docker volume create -d local myvolume

# Create with labels
$ docker volume create -o type=tmpfs myvolume
```

### List Volumes

```bash
# List volumes
$ docker volume ls

# Inspect volume
$ docker volume inspect myvolume

# Get mount path
$ docker volume inspect -f '{{.Mountpoint}}' myvolume

# Filter volumes
$ docker volume ls --filter "name=my"
```

### Remove Volumes

```bash
# Remove volume
$ docker volume rm myvolume

# Remove multiple
$ docker volume rm vol1 vol2

# Remove unused
$ docker volume prune

# Remove all
$ docker volume prune -a
```

---

## System Commands

### Docker Info

```bash
# System information
$ docker info

# Docker version
$ docker version

# Check Docker daemon
$ docker ps

# System-wide statistics
$ docker stats
```

### Cleanup

```bash
# Prune dangling images
$ docker image prune

# Prune stopped containers
$ docker container prune

# Prune unused volumes
$ docker volume prune

# Prune unused networks
$ docker network prune

# Complete cleanup
$ docker system prune

# Complete including tagged
$ docker system prune -a

# Preview cleanup
$ docker system prune --dry-run
```

### Events

```bash
# View Docker events
$ docker events

# Filter events
$ docker events --filter "type=container"
$ docker events --filter "image=nginx"

# Since specific time
$ docker events --since 2024-01-15T10:00:00

# Until specific time
$ docker events --until 2024-01-15T11:00:00
```

---

## Registry Commands

### Docker Hub

```bash
# Login to Docker Hub
$ docker login

# Login to specific registry
$ docker login myregistry.com

# Logout
$ docker logout

# Logout from registry
$ docker logout myregistry.com

# Push image
$ docker push username/image:tag

# Pull image
$ docker pull username/image:tag

# Search images
$ docker search keyword
```

---

## Build Commands

### Dockerfile Building

```bash
# Build image
$ docker build -t myimage:1.0 .

# Build from file
$ docker build -f Dockerfile.prod -t app:1.0 .

# Build with build args
$ docker build --build-arg NODE_ENV=production .

# Build with labels
$ docker build --label version=1.0 -t app:1.0 .

# Build without cache
$ docker build --no-cache -t app:1.0 .

# View build process
$ docker build -t app:1.0 . (verbose output)
```

---

## Utility Commands

### Help

```bash
# General help
$ docker --help

# Help for specific command
$ docker run --help
$ docker build --help

# Docker version info
$ docker --version
$ docker version
```

### Useful Combinations

```bash
# Stop all containers
$ docker stop $(docker ps -q)

# Remove all containers
$ docker rm $(docker ps -a -q)

# Remove all images
$ docker rmi $(docker images -q)

# Get container IP
$ docker inspect -f '{{.NetworkSettings.IPAddress}}' container-id

# Execute command in all containers
$ docker ps -q | xargs -I {} docker exec {} command

# Stream logs from multiple containers
$ docker logs -f container1 & docker logs -f container2
```

---

## Summary 📝

```
IMAGE COMMANDS:
pull, push, build, tag, rmi, images, history, inspect, search

CONTAINER COMMANDS:
run, create, start, stop, restart, kill, rm, ps, logs, exec, cp

NETWORK COMMANDS:
network create, connect, disconnect, rm, ls, inspect

VOLUME COMMANDS:
volume create, rm, ls, inspect, prune

SYSTEM COMMANDS:
info, version, stats, events, system prune
```

---

## 🔗 Next Section

👉 **[07-Dockerfile](../07-Dockerfile/README.md)** - Write Your Own Dockerfiles

---

**Master these commands and you'll be a Docker pro!** 🐳

Last updated: January 2025  
Difficulty: ⭐⭐ Beginner  
Time: 2 hours
