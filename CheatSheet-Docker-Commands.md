# 🐳 Docker Commands Cheatsheet

**Quick Reference for All Docker Commands**

---

## 🖥️ Installation & Setup

```bash
# Check Docker version
$ docker --version
$ docker version

# Check Docker info
$ docker info

# Test installation
$ docker run hello-world

# Update Docker
# Windows/Mac: Docker Desktop > Check for updates
# Linux: sudo apt update && sudo apt upgrade docker.io
```

---

## 🖼️ Image Commands

### Pull & Download

```bash
# Pull image from Docker Hub
$ docker pull image-name
$ docker pull python:3.9
$ docker pull ubuntu:22.04

# Pull from specific registry
$ docker pull myregistry.com/image:tag

# Pull all tags
$ docker pull --all-tags python
```

### List Images

```bash
# List all images
$ docker images

# List with no truncation
$ docker images --no-trunc

# List only image IDs
$ docker images -q

# Filter images
$ docker images --filter "dangling=true"
$ docker images --filter "reference=python*"

# Format output
$ docker images --format "table {{.Repository}}\t{{.Size}}"
```

### Build Images

```bash
# Build image from Dockerfile
$ docker build -t myimage:1.0 .

# Build with specific Dockerfile
$ docker build -f Dockerfile.prod -t myapp:1.0 .

# Build with arguments
$ docker build --build-arg NODE_ENV=production -t app:1.0 .

# Build without cache
$ docker build --no-cache -t myimage:1.0 .

# Build and push
$ docker build -t username/myimage:1.0 .
$ docker push username/myimage:1.0
```

### Tag Images

```bash
# Create alias/tag
$ docker tag image-id myimage:1.0

# Rename image
$ docker tag old-name:tag new-name:tag

# Tag for registry
$ docker tag myimage:1.0 myregistry.com/myimage:1.0
```

### Remove Images

```bash
# Remove image
$ docker rmi image-name
$ docker rmi image-id

# Remove multiple images
$ docker rmi image1 image2 image3

# Remove unused images
$ docker image prune

# Remove all unused images
$ docker image prune -a

# Force remove
$ docker rmi -f image-name
```

### Image Information

```bash
# Inspect image
$ docker inspect image-name

# View image history
$ docker history image-name

# View image layers
$ docker history --no-trunc image-name

# Save image to file
$ docker save image-name > image.tar

# Load image from file
$ docker load < image.tar
```

### Search Images

```bash
# Search Docker Hub
$ docker search python

# Search with filters
$ docker search python --filter "is-official=true"

# Limit results
$ docker search python --limit 5
```

---

## 📦 Container Commands

### Create & Run

```bash
# Run container
$ docker run image-name

# Run in background
$ docker run -d image-name

# Run with name
$ docker run --name my-container image-name

# Run with port mapping
$ docker run -p 8080:80 nginx
$ docker run -p 8080:80 -p 8443:443 nginx

# Run with volume
$ docker run -v /host/path:/container/path image-name
$ docker run -v data:/data image-name

# Run with environment variable
$ docker run -e KEY=value image-name
$ docker run -e ENV=production image-name

# Run with multiple options
$ docker run -d -p 8080:80 -v data:/data \
  -e KEY=value --name my-web nginx

# Run with terminal access
$ docker run -it ubuntu bash
$ docker run -it python:3.9 python

# Run with resource limits
$ docker run -m 512m image-name        # Memory limit
$ docker run --cpus 1.5 image-name     # CPU limit

# Run with restart policy
$ docker run --restart always image-name
$ docker run --restart on-failure:5 image-name

# Run as different user
$ docker run --user 1000 image-name

# Run with hostname
$ docker run --hostname myhost image-name

# Run with working directory
$ docker run -w /app image-name

# Run with network
$ docker run --network mynetwork image-name

# Run linked containers (legacy)
$ docker run --link other-container:alias image-name

# Run with add host
$ docker run --add-host myhost:127.0.0.1 image-name
```

### List Containers

```bash
# List running containers
$ docker ps

# List all containers
$ docker ps -a

# List last created container
$ docker ps -l

# List container IDs only
$ docker ps -q

# Filter containers
$ docker ps --filter "status=running"
$ docker ps --filter "name=web"
$ docker ps --filter "ancestor=nginx"

# Format output
$ docker ps --format "table {{.Names}}\t{{.Status}}"

# Show container ports
$ docker ps --no-trunc
```

### Start/Stop Containers

```bash
# Start container
$ docker start container-id
$ docker start container-name

# Stop container
$ docker stop container-id

# Restart container
$ docker restart container-id

# Kill container (force stop)
$ docker kill container-id

# Pause container
$ docker pause container-id

# Unpause container
$ docker unpause container-id

# Stop all containers
$ docker stop $(docker ps -q)

# Restart all containers
$ docker restart $(docker ps -q)
```

### Remove Containers

```bash
# Remove stopped container
$ docker rm container-id

# Remove running container (force)
$ docker rm -f container-id

# Remove multiple containers
$ docker rm container1 container2 container3

# Remove all stopped containers
$ docker container prune

# Remove all containers
$ docker rm $(docker ps -a -q)
```

### Container Information

```bash
# Inspect container
$ docker inspect container-id

# Get IP address
$ docker inspect -f '{{.NetworkSettings.IPAddress}}' container-id

# View container logs
$ docker logs container-id

# Follow logs (real-time)
$ docker logs -f container-id

# View last 100 lines
$ docker logs --tail 100 container-id

# View with timestamps
$ docker logs -t container-id

# View resource usage
$ docker stats

# View specific container stats
$ docker stats container-id

# View running processes
$ docker top container-id

# View port mappings
$ docker port container-id

# Get container details in format
$ docker inspect -f '{{.Name}}' container-id
```

### Execute Commands

```bash
# Execute command in running container
$ docker exec container-id command

# Get interactive shell
$ docker exec -it container-id bash
$ docker exec -it container-id sh

# Execute as specific user
$ docker exec -u root container-id command

# Execute with environment
$ docker exec -e VAR=value container-id command

# Execute and return output
$ docker exec container-id cat /var/log/app.log
```

### Copy Files

```bash
# Copy from container to host
$ docker cp container-id:/app/file.txt ./

# Copy from host to container
$ docker cp ./file.txt container-id:/app/

# Copy directory
$ docker cp ./local-dir container-id:/app/
```

### Commit Container

```bash
# Create image from container
$ docker commit container-id myimage:1.0

# Create image with message
$ docker commit -m "Added packages" container-id myimage:1.0

# Create image with author
$ docker commit -a "Your Name" container-id myimage:1.0
```

---

## 🌐 Network Commands

### Create Networks

```bash
# Create network
$ docker network create mynetwork

# Create bridge network
$ docker network create -d bridge mynetwork

# Create overlay network
$ docker network create -d overlay mynetwork
```

### List Networks

```bash
# List networks
$ docker network ls

# Inspect network
$ docker network inspect mynetwork
```

### Connect Containers

```bash
# Connect container to network
$ docker network connect mynetwork container-id

# Disconnect container from network
$ docker network disconnect mynetwork container-id
```

### Remove Networks

```bash
# Remove network
$ docker network rm mynetwork

# Remove unused networks
$ docker network prune
```

---

## 💾 Volume Commands

### Create Volumes

```bash
# Create volume
$ docker volume create myvolume

# Create volume with driver
$ docker volume create -d local myvolume
```

### List Volumes

```bash
# List all volumes
$ docker volume ls

# Inspect volume
$ docker volume inspect myvolume

# Find volume location
$ docker volume inspect -f '{{.Mountpoint}}' myvolume
```

### Remove Volumes

```bash
# Remove volume
$ docker volume rm myvolume

# Remove unused volumes
$ docker volume prune

# Remove all volumes
$ docker volume prune -a
```

---

## 📁 Dockerfile Commands

### Common Dockerfile Instructions

```dockerfile
FROM ubuntu:22.04                    # Base image
WORKDIR /app                         # Set working directory
COPY . .                             # Copy files
ADD file.tar /app/                   # Copy & extract
RUN apt-get update                   # Run command
ENV KEY=value                        # Environment variable
EXPOSE 8080                          # Expose port
VOLUME /data                         # Define volume
ENTRYPOINT ["python", "app.py"]     # Default command
CMD ["--help"]                       # Default arguments
USER appuser                         # Switch user
LABEL version="1.0"                  # Metadata
```

### Build Dockerfile

```bash
# Build from Dockerfile
$ docker build -t myimage:1.0 .

# Build from specific file
$ docker build -f Dockerfile.prod -t app:1.0 .

# Build with arguments
$ docker build --build-arg VERSION=1.0 .

# Build without cache
$ docker build --no-cache .

# View build output
$ docker build -t image:1.0 .
```

---

## 🐳 Docker Compose Commands

### Compose Operations

```bash
# Start services
$ docker-compose up

# Start in background
$ docker-compose up -d

# Stop services
$ docker-compose down

# View services
$ docker-compose ps

# View logs
$ docker-compose logs

# Follow logs
$ docker-compose logs -f

# View service logs
$ docker-compose logs service-name

# Restart services
$ docker-compose restart

# Build images
$ docker-compose build

# Build and start
$ docker-compose up --build

# Execute command
$ docker-compose exec service-name command

# Scale services
$ docker-compose up --scale web=3

# Remove services and volumes
$ docker-compose down -v

# Pause services
$ docker-compose pause

# Unpause services
$ docker-compose unpause

# Stop without removing
$ docker-compose stop

# Start existing
$ docker-compose start

# List containers
$ docker-compose ps -a
```

---

## 📤 Registry Commands

### Push to Registry

```bash
# Login to Docker Hub
$ docker login

# Logout
$ docker logout

# Push image
$ docker push username/image:tag

# Push to private registry
$ docker push myregistry.com/image:tag

# Tag and push
$ docker tag myimage:latest username/myimage:latest
$ docker push username/myimage:latest
```

### Pull from Registry

```bash
# Pull from Docker Hub
$ docker pull image-name

# Pull specific tag
$ docker pull image-name:tag

# Pull from private registry
$ docker pull myregistry.com/image:tag
```

---

## 🧹 Cleanup Commands

### Clean Up

```bash
# Remove dangling images
$ docker image prune

# Remove dangling images & unused
$ docker image prune -a

# Remove stopped containers
$ docker container prune

# Remove unused volumes
$ docker volume prune

# Remove unused networks
$ docker network prune

# Remove all unused
$ docker system prune

# Remove all (including tagged images)
$ docker system prune -a

# See what will be removed
$ docker system prune --dry-run
```

---

## 🔍 Debugging Commands

### Troubleshooting

```bash
# View Docker events (real-time)
$ docker events

# Check Docker daemon logs
$ docker logs daemon          # Linux
# Mac: ~/Library/Logs/Docker.log
# Windows: AppData\Local\Docker\log.txt

# Inspect container
$ docker inspect container-id

# Check image vulnerabilities
$ docker scan image-name

# View container details
$ docker container ls -a

# Check network connectivity
$ docker network inspect bridge

# Test image
$ docker run --rm image-name /bin/sh
```

---

## 🎯 Quick Commands Summary

| Task | Command |
|------|---------|
| See available commands | `docker --help` |
| Get help for command | `docker COMMAND --help` |
| Version info | `docker --version` |
| System info | `docker info` |
| Pull image | `docker pull image` |
| Run container | `docker run image` |
| List containers | `docker ps -a` |
| Stop container | `docker stop container` |
| Remove container | `docker rm container` |
| View logs | `docker logs container` |
| Build image | `docker build -t image .` |
| Tag image | `docker tag old new` |
| Push image | `docker push user/image` |
| Run compose | `docker-compose up` |
| Clean up | `docker system prune` |

---

## 📝 Tips & Tricks

```bash
# Get container ID from name
$ docker ps -aqf "name=container-name"

# Get IP address
$ docker inspect -f '{{.NetworkSettings.IPAddress}}' container

# Copy from container
$ docker cp container:/path/file .

# View last 10 commands
$ history | tail -10

# Run multiple commands
$ docker exec container bash -c "cmd1 && cmd2 && cmd3"

# Tail logs with grep
$ docker logs -f container | grep ERROR

# Interactive shell in container
$ docker run -it image bash

# Remove all stopped containers
$ docker container prune -f

# See container environment
$ docker exec container env

# Monitor containers real-time
$ docker stats --no-stream

# Get container port info
$ docker port container
```

---

## 🚀 Useful Aliases

```bash
# Add these to ~/.bashrc or ~/.zshrc

# List commands
alias dl='docker ps -l -q'
alias di='docker images'
alias dps='docker ps'
alias dpa='docker ps -a'

# Container operations
alias dstart='docker start'
alias dstop='docker stop'
alias drm='docker rm'
alias drmi='docker rmi'

# Shortcuts
alias docker-shell='docker run -it --rm'
alias docker-logs='docker logs -f'

# Cleanup
alias docker-clean='docker system prune -a'
```

---

**Save this for quick reference!** 📌

Last updated: January 2025  
Bookmark this page! 🔖
