# 05 - Docker Containers 📦

**Running and Managing Containers - Practical Guide**

---

## 📚 Table of Contents

1. [What is a Container?](#what-is-a-container)
2. [Container Lifecycle](#container-lifecycle)
3. [Running Containers](#running-containers)
4. [Managing Containers](#managing-containers)
5. [Container Commands](#container-commands)
6. [Logs & Debugging](#logs--debugging)
7. [Real Examples](#real-examples)
8. [Summary](#summary)

---

## What is a Container?

### Simple Definition 🎯

**A container is a running instance of an image - like a live application**

```
Container = Image in action
├─ Actively running
├─ Using resources (CPU, RAM)
├─ Can be started/stopped
├─ Has its own isolated environment
└─ Temporary (can be deleted)

Think of it like:
├─ Image = Frozen food package
├─ Container = Heating up that food
└─ Result = Ready to eat! 🍽️
```

### Container Features 🎁

```
Each container has:
├─ Isolated file system
│  └─ Can't see other container files
├─ Isolated network (by default)
│  └─ Own IP address
├─ Isolated process space
│  └─ Process ID (PID) 1 = main app
├─ Own environment variables
│  └─ Configuration specific to container
├─ Resource limits (optional)
│  └─ Max CPU, RAM, disk
└─ Own hostname
   └─ Container ID used as hostname
```

### Visual Representation 🎨

```
RUNNING CONTAINER:

┌──────────────────────────────────┐
│ 🐳 CONTAINER                     │
├──────────────────────────────────┤
│ ID: a7f3d2e1c9b                  │
│ Name: my-web-server              │
│ Status: Running (3 minutes)       │
│ Image: nginx:latest              │
│                                  │
│ Resources:                        │
│ ├─ CPU: 0.05%                    │
│ ├─ Memory: 12 MB                 │
│ └─ Port: 8080:80                 │
│                                  │
│ File System:                      │
│ ├─ /app/                         │
│ ├─ /etc/                         │
│ └─ /var/                         │
│                                  │
│ Running Process:                  │
│ ├─ PID 1: nginx                  │
│ └─ PID 2-5: worker threads       │
└──────────────────────────────────┘
```

---

## Container Lifecycle

### States of a Container 🔄

```
CREATE
  │
  ▼
STARTED
  │
  ├─ RUNNING ◄─────────┐
  │    │               │
  │    ▼               │
  │  PAUSED            │
  │    │      UNPAUSE──┘
  │    PAUSE
  │
  ▼
STOPPED
  │
  ├─ RESTART ──────────► RUNNING
  │
  ▼
REMOVED
  │
  (Deleted)
```

### State Transitions 📊

```
CREATED → Not started yet
├─ Container exists but not running
├─ Consumes disk space only
└─ Use: docker create

RUNNING → App is executing
├─ Actively using resources
├─ Can receive requests
├─ Use: docker start / docker run

PAUSED → Frozen temporarily
├─ Processes suspended
├─ Still in memory
├─ Use: docker pause

STOPPED → Container halted
├─ Not running, but exists
├─ Can be restarted
├─ Use: docker stop

REMOVED → Completely deleted
├─ Container doesn't exist
├─ Frees up all resources
├─ Use: docker rm
```

### Timeline Example ⏱️

```
$ docker run nginx

TIME 1 (0s): CREATE
└─ Container created from nginx image

TIME 2 (0.5s): START
└─ Docker starts container

TIME 3 (1s): RUNNING
├─ Nginx server starting
└─ Listening on port 80

TIME 4-100s: RUNNING
└─ Nginx serving requests

TIME 101s: STOP COMMAND
$ docker stop container-id

TIME 102s: STOPPED
├─ Nginx shutdown
└─ Container halted

TIME 103-1000s: STOPPED (resting)
└─ Container idle but exists

TIME 1001s: RESTART
$ docker start container-id

TIME 1002s: RUNNING AGAIN
└─ Nginx starts again

TIME 1003s: REMOVE
$ docker rm container-id

TIME 1004s: REMOVED
└─ Container deleted forever
```

---

## Running Containers

### Basic docker run 🚀

```bash
# Simplest form
$ docker run image-name

# Most useful form
$ docker run -d -p 8080:80 --name my-web nginx

Breakdown:
-d              Detach (background)
-p 8080:80      Port mapping (host:container)
--name          Give container a name
nginx           Image to use
```

### Understanding -d (Detach) 🎭

```
WITHOUT -d (Attached):
$ docker run nginx
├─ Container runs in foreground
├─ You see all output
├─ Ctrl+C stops container
└─ Terminal blocked

WITH -d (Detached):
$ docker run -d nginx
├─ Container runs in background
├─ Returns container ID
├─ You get terminal back
└─ Container keeps running
```

### Understanding -p (Port Mapping) 🚪

```
What is port mapping?
├─ Container has port (inside)
├─ Computer has port (outside)
├─ Mapping = Connection between them

Syntax: -p host-port:container-port

Examples:
$ docker run -p 8080:80 nginx
  └─ Port 8080 on your PC → Port 80 in container

$ docker run -p 3306:3306 mysql
  └─ Port 3306 on your PC → Port 3306 in container

$ docker run -p 5000:5000 myapp
  └─ Port 5000 on your PC → Port 5000 in container

Visual:
YOUR COMPUTER          CONTAINER
Port 8080         ──► Port 80 (nginx)
Port 3306         ──► Port 3306 (MySQL)
Port 5000         ──► Port 5000 (Flask)
```

### Common Run Options 🛠️

```bash
# Run with name
$ docker run --name my-container image

# Run in background
$ docker run -d image

# Map ports
$ docker run -p 8080:80 image

# Set environment variables
$ docker run -e KEY=value image

# Mount volume
$ docker run -v /host/path:/container/path image

# Limit resources
$ docker run -m 512m image  # 512 MB RAM limit

# Run with terminal access
$ docker run -it image bash

# Run as different user
$ docker run --user 1000 image

# Add hostname
$ docker run --hostname myhost image

# Restart policy
$ docker run --restart always image
```

---

## Managing Containers

### List Containers 📋

```bash
# List running containers
$ docker ps

# List all containers (running + stopped)
$ docker ps -a

# List with more details
$ docker ps -l  # Last container

# Show only IDs
$ docker ps -q

# Filter containers
$ docker ps -f "status=running"
$ docker ps -f "name=web"
```

### Start/Stop Containers ▶️⏹️

```bash
# Start container
$ docker start container-id

# Stop container (graceful)
$ docker stop container-id

# Restart container
$ docker restart container-id

# Kill container (force stop)
$ docker kill container-id

# Pause container (freeze)
$ docker pause container-id

# Unpause container (resume)
$ docker unpause container-id
```

### Remove Containers 🗑️

```bash
# Remove stopped container
$ docker rm container-id

# Remove running container (force)
$ docker rm -f container-id

# Remove all stopped containers
$ docker container prune

# Remove specific by name
$ docker rm my-container
```

### Get Container Info 🔍

```bash
# View container details
$ docker inspect container-id

# View resource usage
$ docker stats

# View running processes inside
$ docker top container-id

# View port mappings
$ docker port container-id
```

---

## Container Commands

### Essential Commands Reference 📚

```bash
# RUN - Create and start container
$ docker run [OPTIONS] IMAGE [COMMAND]

# CREATE - Create but don't start
$ docker create [OPTIONS] IMAGE

# START - Start stopped container
$ docker start CONTAINER

# STOP - Stop running container
$ docker stop CONTAINER

# RESTART - Restart container
$ docker restart CONTAINER

# KILL - Force stop container
$ docker kill CONTAINER

# PAUSE - Freeze container
$ docker pause CONTAINER

# UNPAUSE - Resume container
$ docker unpause CONTAINER

# REMOVE - Delete container
$ docker rm CONTAINER

# PS - List containers
$ docker ps [OPTIONS]

# INSPECT - Show details
$ docker inspect CONTAINER

# STATS - Resource usage
$ docker stats

# TOP - View processes
$ docker top CONTAINER

# PORT - Show port mappings
$ docker port CONTAINER

# RENAME - Rename container
$ docker rename OLD-NAME NEW-NAME

# COMMIT - Create image from container
$ docker commit CONTAINER

# EXPORT - Export container
$ docker export CONTAINER

# IMPORT - Import container
$ docker import

# PRUNE - Clean up stopped containers
$ docker container prune
```

---

## Logs & Debugging

### View Container Logs 📜

```bash
# View all logs
$ docker logs container-id

# View last 50 lines
$ docker logs --tail 50 container-id

# Follow logs in real-time (like tail -f)
$ docker logs -f container-id

# Show timestamps
$ docker logs -t container-id

# View since specific time
$ docker logs --since 2024-01-15T10:00:00 container-id

# Previous container logs (if restarted)
$ docker logs --previous container-id
```

### Execute Commands in Container 🖥️

```bash
# Run command in running container
$ docker exec container-id COMMAND

# Get interactive shell
$ docker exec -it container-id bash

# Run as root
$ docker exec -u root container-id command

# Examples:
$ docker exec my-web ls /app
$ docker exec my-db mysql -u root -p
$ docker exec nginx nginx -s reload
```

### Debug Container 🐛

```bash
# Check if container is running
$ docker ps | grep my-container

# Get detailed information
$ docker inspect my-container

# View resource usage
$ docker stats my-container

# Check recent logs
$ docker logs --tail 100 my-container

# Get shell access
$ docker exec -it my-container bash

# Check network
$ docker network inspect bridge

# View processes inside
$ docker top my-container
```

---

## Real Examples

### Example 1: Web Server 🌐

```bash
# Run Nginx web server
$ docker run -d -p 8080:80 --name my-web nginx

# Check if running
$ docker ps

# Visit http://localhost:8080
# You should see Nginx page!

# View logs
$ docker logs my-web

# Stop it
$ docker stop my-web

# Start again
$ docker start my-web

# Remove it
$ docker rm my-web
```

### Example 2: Database 🗄️

```bash
# Run MySQL
$ docker run -d \
  --name my-db \
  -e MYSQL_ROOT_PASSWORD=secret \
  -p 3306:3306 \
  mysql:8.0

# Check running
$ docker ps

# Connect to database
$ docker exec -it my-db mysql -u root -p

# Stop database
$ docker stop my-db

# Start again
$ docker start my-db

# View logs
$ docker logs my-db

# Remove
$ docker rm my-db
```

### Example 3: Python App 🐍

```bash
# Run Python container with shell
$ docker run -it python:3.9 bash

# Inside container, you can run Python
root@container:/ python3
>>> print("Hello from Docker!")
Hello from Docker!
>>> exit()

# Run Python script
$ docker run python:3.9 python -c "print('Hello')"

# Run specific script (if exists in image)
$ docker run python:3.9 python /app/script.py
```

### Example 4: Multiple Containers 🐳🐳🐳

```bash
# Start web server
$ docker run -d --name web -p 8080:80 nginx

# Start database
$ docker run -d --name db -e MYSQL_ROOT_PASSWORD=pass mysql:8.0

# Start cache
$ docker run -d --name cache -p 6379:6379 redis

# List all
$ docker ps

# Stop all
$ docker stop web db cache

# Remove all
$ docker rm web db cache
```

---

## Summary 📝

```
KEY CONCEPTS:

✅ Container = Running image instance
✅ Lifecycle = Create → Start → Running → Stop → Remove
✅ Port mapping = Connect container port to host
✅ Detach mode = Run in background
✅ Logs = Debug by viewing output
✅ Execute = Run commands in container

WORKFLOW:
1. Pull image: docker pull image
2. Run container: docker run -d image
3. Check status: docker ps
4. View logs: docker logs container
5. Stop: docker stop container
6. Remove: docker rm container
```

---

## 🔗 Next Section

👉 **[06-Docker-Commands](../06-Docker-Commands/README.md)** - Complete Docker Commands Reference

---

## 📚 Key Takeaways

1. **Container** = Running app instance
2. **Run** = Start new container
3. **Port mapping** = Access container from computer
4. **Logs** = See what container is doing
5. **Lifecycle** = Create, Start, Stop, Remove
6. **Exec** = Run commands inside container

---

**Ready for complete commands reference?** Let's go! 🚀

Last updated: January 2025  
Difficulty: ⭐⭐ Beginner  
Time: 1.5 hours
