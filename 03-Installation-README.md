# 03 - Installing Docker 💻

**Get Docker running on your computer in 5 minutes!**

---

## 📚 Table of Contents

1. [Installation Overview](#installation-overview)
2. [Install on Windows](#install-on-windows)
3. [Install on Mac](#install-on-mac)
4. [Install on Linux](#install-on-linux)
5. [Verify Installation](#verify-installation)
6. [First Container](#first-container)
7. [Troubleshooting](#troubleshooting)

---

## Installation Overview

### What We're Installing 📦

```
Docker has two editions:

1) DOCKER DESKTOP (Recommended for beginners)
   ├─ GUI interface (Click-friendly)
   ├─ Works on Windows/Mac
   ├─ Includes all tools
   ├─ One-click installation
   └─ FREE!

2) DOCKER ENGINE (For Linux servers)
   ├─ Command line only
   ├─ Lightweight
   ├─ For Linux machines
   └─ FREE!

Which one? → Start with Docker Desktop!
```

### System Requirements ⚙️

```
MINIMUM (Okay performance):
├─ 4 GB RAM
├─ 10 GB Disk space
├─ Modern processor
└─ Windows 10/11, Mac, or Linux

RECOMMENDED (Good performance):
├─ 8+ GB RAM
├─ 20+ GB Disk space
├─ Multi-core processor
└─ Windows 10/11, Mac, or Linux

Check your specs:
Windows: Settings > System > About
Mac: Apple menu > About This Mac
Linux: cat /proc/meminfo
```

---

## Install on Windows

### Step 1: Download Docker Desktop 📥

```
1. Go to: https://www.docker.com/products/docker-desktop
2. Click: "Docker Desktop for Windows"
3. Click: Download
4. File will download: DockerDesktopInstaller.exe
```

### Step 2: Run Installer 🔧

```
1. Double-click DockerDesktopInstaller.exe
2. Installation wizard opens
3. Keep default settings
4. Click: Install
5. Accept admin rights when asked
6. Wait for installation (1-2 minutes)
7. Click: Finish
8. Restart your computer (if asked)
```

### Step 3: Verify Installation ✅

```
After restart, open Command Prompt:
Windows Key + R
Type: cmd
Press: Enter

In Command Prompt, type:
$ docker --version

You should see:
Docker version 24.0.0, build xyz

Great! Docker is installed! ✅
```

### Step 4: First Command 🐳

```
In same Command Prompt:
$ docker run hello-world

You will see:
├─ Hello from Docker!
├─ This message shows Docker works
└─ Congratulations!

This means Docker is working! 🎉
```

### Troubleshooting Windows

```
ERROR: "docker not found"
Solution:
├─ Restart computer
├─ Check Docker Desktop is running
│  (Look for Docker icon in taskbar)
└─ Reinstall if needed

ERROR: "WSL 2 required"
Solution:
├─ Enable Windows Subsystem for Linux 2 (WSL 2)
├─ Windows Key + Turn Windows features on/off
├─ Check: Windows Subsystem for Linux
├─ Check: Virtual Machine Platform
├─ Restart computer
└─ Retry Docker installation

ERROR: "Docker Desktop won't start"
Solution:
├─ Restart Docker Desktop
├─ Update Windows
├─ Uninstall and reinstall Docker
└─ Check system resources (RAM, disk space)
```

---

## Install on Mac

### Step 1: Download Docker Desktop 📥

```
1. Go to: https://www.docker.com/products/docker-desktop
2. Choose correct version:
   ├─ If Mac has Intel processor: "Docker Desktop for Mac (Intel)"
   ├─ If Mac has Apple Silicon (M1/M2): "Docker Desktop for Mac (Apple Silicon)"
3. Click: Download
4. File will download: Docker.dmg
```

**How to check your Mac processor:**
```
Apple menu > About This Mac
Look for "Processor" or "Chip"
```

### Step 2: Run Installer 🔧

```
1. Double-click Docker.dmg
2. Drag Docker icon to Applications folder
3. Wait for copy operation (1-2 minutes)
4. Docker will appear in Applications folder
5. Done! Installer is closed
```

### Step 3: Launch Docker 🚀

```
1. Go to: Applications folder
2. Find: Docker
3. Double-click: Docker.app
4. Docker starts (you'll see whale icon)
5. First launch takes 1-2 minutes
6. When ready, whale icon stops animating
```

### Step 4: Verify Installation ✅

```
Open Terminal:
Command + Space
Type: terminal
Press: Enter

In Terminal, type:
$ docker --version

You should see:
Docker version 24.0.0, build xyz

Perfect! Docker is installed! ✅
```

### Step 5: First Command 🐳

```
In same Terminal:
$ docker run hello-world

You will see:
├─ Hello from Docker!
├─ This message shows Docker works
└─ Congratulations!

This means Docker is working! 🎉
```

### Troubleshooting Mac

```
ERROR: "docker: command not found"
Solution:
├─ Docker Desktop might not be running
├─ Click whale icon in menu bar
├─ Wait for "Docker is running"
└─ Try command again

ERROR: "Permission denied"
Solution:
├─ Use sudo (with caution):
└─ $ sudo docker run hello-world

ERROR: "Docker won't start"
Solution:
├─ Restart Mac
├─ Check available RAM (at least 4GB)
├─ Update Docker Desktop to latest version
└─ Uninstall and reinstall if needed

ERROR: "Out of disk space"
Solution:
├─ Free up disk space (at least 10GB)
├─ Docker needs space for images/containers
└─ Delete old files or use external drive
```

---

## Install on Linux

### Ubuntu/Debian Installation 📥

```
Step 1: Update package manager
$ sudo apt update
$ sudo apt upgrade

Step 2: Install Docker
$ sudo apt install docker.io

Step 3: Verify installation
$ docker --version

Step 4: Test Docker
$ sudo docker run hello-world

Step 5: Allow non-root user (Optional but recommended)
$ sudo usermod -aG docker $USER
$ newgrp docker

Now you can use docker without sudo!
```

### CentOS/RHEL Installation 📥

```
Step 1: Install Docker
$ sudo yum install docker

Step 2: Start Docker service
$ sudo systemctl start docker

Step 3: Verify installation
$ docker --version

Step 4: Test Docker
$ sudo docker run hello-world

Step 5: Make Docker auto-start
$ sudo systemctl enable docker
```

### Troubleshooting Linux

```
ERROR: "docker: command not found"
Solution:
├─ Reinstall: sudo apt install docker.io
├─ Or: sudo yum install docker
└─ Verify path: which docker

ERROR: "Permission denied"
Solution 1 (Temporary):
├─ Use sudo: sudo docker run hello-world

Solution 2 (Permanent):
├─ Add user to docker group:
├─ $ sudo usermod -aG docker $USER
├─ Logout and login
├─ No more sudo needed!

ERROR: "Cannot connect to Docker daemon"
Solution:
├─ Start Docker:
├─ $ sudo systemctl start docker
├─ Enable auto-start:
└─ $ sudo systemctl enable docker
```

---

## Verify Installation

### Complete Verification ✅

```bash
# Check Docker version
$ docker --version

# Check Docker system info
$ docker info

# Run hello-world container
$ docker run hello-world

# List images
$ docker images

# List containers
$ docker ps -a
```

### What You Should See 📊

```
$ docker --version
Docker version 24.0.0, build 3713ee1

$ docker info
Client:
 Version: 24.0.0
 ...

Server:
 OS/Arch: linux/x86_64
 Containers: 1
 Images: 1
 ...

$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
...
Hello from Docker!
This message shows Docker works correctly.
```

### Docker Desktop Status 🐳

**On Windows/Mac:**
```
Look for Docker whale icon in:
├─ Taskbar (Windows) - bottom right
├─ Menu bar (Mac) - top right
├─ Icon should be animated (running)
├─ Click icon → "Docker is running" ✅
└─ If not running, click to start
```

---

## First Container

### The "Hello World" Container 👋

```
What happens when you run:
$ docker run hello-world

STEP 1: Docker Client parses command
STEP 2: Docker Daemon checks for hello-world image
STEP 3: Image not found locally
STEP 4: Daemon pulls from Docker Hub
STEP 5: Image downloaded (~9 KB)
STEP 6: Container created and started
STEP 7: Container runs, prints message
STEP 8: Container exits
STEP 9: Message shows on your screen

Result: Hello from Docker! ✅
```

### Run Your First Real Container 🌐

```bash
# Run an Nginx web server
$ docker run -d -p 8080:80 --name my-web nginx

Explanation:
-d          = Run in background
-p 8080:80  = Map port 8080 (your computer) 
              to 80 (container)
--name      = Give container a name
nginx       = Image to use

# Check if running
$ docker ps

# Visit in browser
http://localhost:8080

You should see nginx welcome page! 🎉

# Stop container
$ docker stop my-web

# Start it again
$ docker start my-web

# Remove container
$ docker rm my-web
```

### Container Lifecycle 🔄

```
┌────────────┐
│   CREATE   │  docker run / docker create
└─────┬──────┘
      │
      ▼
┌────────────┐
│   START    │  docker start
└─────┬──────┘
      │
      ▼
┌────────────┐
│  RUNNING   │  Container is active
└─────┬──────┘
      │
      ▼
┌────────────┐
│   STOP     │  docker stop
└─────┬──────┘
      │
      ▼
┌────────────┐
│  EXITED    │  Container stopped but exists
└─────┬──────┘
      │
      ▼
┌────────────┐
│   REMOVE   │  docker rm
└─────┬──────┘
      │
      ▼
┌────────────┐
│  REMOVED   │  Container deleted
└────────────┘
```

---

## Troubleshooting

### Common Issues & Solutions 🔧

| Issue | Cause | Solution |
|-------|-------|----------|
| "docker: command not found" | Docker not installed | Reinstall Docker Desktop |
| "Cannot connect to daemon" | Docker not running | Start Docker Desktop |
| "Port already in use" | Port 8080 occupied | Use different port: `-p 9090:80` |
| "Out of disk space" | Not enough space | Free up disk or delete images |
| "Permission denied" | User not in docker group | Add user: `sudo usermod -aG docker $USER` |
| "Image not found" | Image doesn't exist | Pull image first: `docker pull imagename` |
| "Container exited" | App crashed | Check logs: `docker logs container-id` |

### Quick Diagnostics 🔍

```bash
# Check Docker is running
$ docker ps

# See Docker version and info
$ docker version
$ docker info

# Check available images
$ docker images

# Check all containers (running and stopped)
$ docker ps -a

# View container logs
$ docker logs container-id

# Get more details about container
$ docker inspect container-id
```

---

## Next Steps 🚀

```
Congratulations! Docker is installed! 🎉

What's next?
├─ Learn images: 04-Images/README.md
├─ Learn containers: 05-Containers/README.md
├─ Learn commands: 06-Docker-Commands/README.md
└─ Build your own: 07-Dockerfile/README.md
```

---

## Summary 📝

```
✅ Docker Desktop installed
✅ Verified working
✅ Ran first container
✅ Ready to learn Docker!

Key installations:
├─ Windows: Docker Desktop for Windows
├─ Mac: Docker Desktop for Mac
├─ Linux: docker.io or docker-ce
└─ All: Same Docker engine inside
```

---

## 🔗 Next Section

👉 **[04-Images](../04-Images/README.md)** - Understand Docker Images

---

**Ready for the next lesson?** Let's learn about Docker images! 🐳

Last updated: January 2025  
Difficulty: ⭐ Beginner  
Time: 30 minutes
