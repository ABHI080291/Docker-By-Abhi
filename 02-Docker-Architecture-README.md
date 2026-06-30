# 02 - Docker Architecture 🏗️

**How Docker works behind the scenes - The Engine explained!**

---

## 📚 Table of Contents

1. [Docker Architecture Overview](#docker-architecture-overview)
2. [Docker Components](#docker-components)
3. [How Docker Works](#how-docker-works)
4. [Docker Daemon](#docker-daemon)
5. [Docker Client](#docker-client)
6. [Communication Flow](#communication-flow)
7. [Storage](#storage)
8. [Summary](#summary)

---

## Docker Architecture Overview

### The Big Picture 🎯

```
┌─────────────────────────────────────────────────────────┐
│              YOUR COMPUTER                              │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  ┌──────────────┐         ┌──────────────────────┐    │
│  │ DOCKER CLIENT│         │   DOCKER DAEMON      │    │
│  │ (CLI/GUI)    │◄───────►│ (Engine/Server)      │    │
│  │              │  Socket │                      │    │
│  └──────────────┘ Unix/TCP└──────────────────────┘    │
│       ▲                           ▲                     │
│       │                           │                     │
│   You type          Manages:      │                     │
│   commands          ├─ Images     │                     │
│                     ├─ Containers │                     │
│                     ├─ Networks   │                     │
│                     └─ Storage    │                     │
│                                   │                     │
│                    ┌──────────────┴───────────┐         │
│                    ▼                          ▼         │
│              ┌──────────┐          ┌────────────────┐  │
│              │  IMAGES  │          │   CONTAINERS   │  │
│              │ (Stored) │          │    (Running)   │  │
│              └──────────┘          └────────────────┘  │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### Simple Explanation 💡

Think of Docker like a Restaurant:

```
🍽️ RESTAURANT ANALOGY

Docker Client = Customer
├─ You (ordering)
├─ "Give me a coffee"
└─ Wait for result

Docker Daemon = Chef + Kitchen
├─ Receives order
├─ Prepares food
├─ Manages resources
└─ Delivers result

Container = Cooked Dish
├─ Result of order
├─ Ready to use
└─ Running now

Image = Recipe
├─ Template
├─ Reusable
└─ Stores how to make dish
```

---

## Docker Components

### 1️⃣ **Docker Client** 👨‍💻

**What:** The tool you use to talk to Docker

```
You interact with:
├─ Docker CLI (Command Line)
│  └─ docker run, docker build, etc.
├─ Docker GUI (Docker Desktop)
│  └─ Click buttons
└─ APIs (Programs)
   └─ Programmatic access

All send commands to Docker Daemon!
```

### 2️⃣ **Docker Daemon** ⚙️

**What:** The actual Docker engine/server

```
Docker Daemon does:
├─ Listens for commands from client
├─ Builds images (docker build)
├─ Runs containers (docker run)
├─ Manages networks
├─ Manages volumes/storage
├─ Pulls/pushes images
└─ Stops/starts containers

Think: Backend server that does all work
```

### 3️⃣ **Docker Registry** 📦

**What:** Storage for images (like GitHub for images)

```
Types:

A) Docker Hub (Public)
   ├─ Official images (Python, Node, MySQL, etc.)
   ├─ Community images
   └─ Millions of images available

B) Private Registry
   ├─ Your own server
   ├─ Private images
   └─ Used by companies

C) Cloud Registries
   ├─ AWS ECR
   ├─ Google GCR
   ├─ Azure ACR
   └─ Your images on cloud
```

### 4️⃣ **Docker Image** 🖼️

**What:** Blueprints/templates for containers

```
Image contains:
├─ Operating System (minimal Linux)
├─ Application code
├─ Libraries and dependencies
├─ Configuration files
├─ Environment variables
└─ Startup instructions

Size: Usually 10MB - 1GB
```

### 5️⃣ **Docker Container** 📦

**What:** Running instance of an image

```
Container is:
├─ Actually running application
├─ Isolated from other containers
├─ Has its own:
│  ├─ File system
│  ├─ Network interface
│  ├─ Process space
│  └─ Environment variables
└─ Can be started/stopped/deleted
```

---

## How Docker Works

### Step-by-Step Workflow 🔄

```
STEP 1: You write Dockerfile
┌──────────────────┐
│ FROM python:3.9  │
│ RUN pip install  │
│ COPY code        │
│ CMD python app   │
└──────────────────┘
          ▼
STEP 2: Build Image
┌──────────────────┐
│ docker build     │
│ Reads Dockerfile │
│ Creates image    │
└──────────────────┘
          ▼
STEP 3: Image Created ✅
┌──────────────────┐
│   IMAGE          │
│ (Blueprint ready)│
└──────────────────┘
          ▼
STEP 4: Run Container
┌──────────────────┐
│ docker run       │
│ Creates container│
│ Starts app       │
└──────────────────┘
          ▼
STEP 5: Container Running ✅
┌──────────────────┐
│   CONTAINER      │
│  (App running)   │
│  PID: 1234       │
│  Status: UP      │
└──────────────────┘
```

### Visual Flow 📊

```
YOUR CODE
    ↓
[Write Dockerfile]
    ↓
docker build  ◄─── BUILD PHASE
    ↓
[Docker Image Created]  🖼️
    ↓
docker run    ◄─── RUN PHASE
    ↓
[Container Running]  🐳
    ↓
APP WORKING ✅
```

---

## Docker Daemon

### What is Docker Daemon? 🤔

```
Docker Daemon = Background service
├─ Runs on your computer
├─ Always listening
├─ Processes commands from client
└─ Manages containers/images

Like:
├─ Background worker
├─ Waits for orders
├─ Executes them
└─ Reports back
```

### How it runs 🏃

```
On Linux:
├─ Runs as privileged process
├─ Direct access to kernel
├─ Native containers
└─ Maximum performance

On Windows/Mac:
├─ Runs in lightweight VM
├─ VM runs Linux
├─ Linux daemon runs containers
└─ Still very fast!

Visual:

WINDOWS/MAC:
┌──────────────────────────┐
│ Your Computer            │
├──────────────────────────┤
│ Docker Desktop           │
│ └─ Lightweight VM (Linux)│
│    └─ Docker Daemon      │
│       └─ Containers      │
└──────────────────────────┘

LINUX:
┌──────────────────────────┐
│ Your Linux Machine       │
├──────────────────────────┤
│ Docker Daemon (native)   │
│ └─ Containers            │
└──────────────────────────┘
```

---

## Docker Client

### How You Interact 💬

```
DOCKER CLIENT:
├─ CLI (Command Line Interface)
│  └─ docker run, docker build, etc.
├─ Docker Desktop (GUI)
│  └─ Visual interface
├─ Docker API (Programmatic)
│  └─ Developers use this
└─ Third-party tools
   └─ Any tool using Docker API

All talk to same Daemon!
```

### Communication Protocol 🔌

```
Communication Methods:

1) Unix Socket (Linux)
   ├─ Local communication only
   ├─ Very fast
   └─ Default on Linux

2) TCP Socket (Remote)
   ├─ Network communication
   ├─ Can be remote
   └─ Used on Docker Desktop

Example:
docker run myimage
    ↓
[Docker Client sends command]
    ↓
[Daemon receives via socket]
    ↓
[Daemon executes]
    ↓
[Returns result to client]
```

---

## Communication Flow

### Complete Request-Response Cycle 🔄

```
YOU TYPE:
$ docker run -p 8080:80 nginx

    ↓ DOCKER CLIENT PARSES COMMAND
    └─ Parse arguments
    └─ Validate options
    └─ Create request

    ↓ SEND TO DOCKER DAEMON
    └─ Via Unix/TCP socket
    └─ REST API call

    ↓ DOCKER DAEMON RECEIVES
    └─ Check if image exists
    └─ If not, pull from registry
    └─ Create container
    └─ Setup networking
    └─ Start container
    └─ Setup port mapping

    ↓ CONTAINER STARTS
    └─ nginx process starts
    └─ Port 8080 mapped to 80

    ↓ RESPONSE BACK TO CLIENT
    └─ Container ID returned
    └─ "Container started successfully"

    ↓ YOU SEE:
    $ docker run -p 8080:80 nginx
    7f3a8e2c1b9d
    Container started! ✅
```

### Detailed Interaction 📡

```
┌─────────────────┐
│   YOU (User)    │
│   $ docker run  │
└────────┬────────┘
         │
         ▼
┌─────────────────────────────┐
│  DOCKER CLIENT              │
│  ├─ Parses command          │
│  ├─ Validates arguments     │
│  └─ Creates API request     │
└────────┬────────────────────┘
         │ (REST API call)
         │ http://localhost/containers/create
         ▼
    [UNIX SOCKET / TCP]
         │
         ▼
┌──────────────────────────────┐
│  DOCKER DAEMON               │
│  ├─ Receives request         │
│  ├─ Validates inputs         │
│  ├─ Checks image existence   │
│  ├─ Creates container        │
│  ├─ Allocates resources      │
│  ├─ Starts container         │
│  └─ Returns container info   │
└────────┬─────────────────────┘
         │ (Response)
         │ Container ID + Status
         ▼
┌─────────────────────┐
│  DOCKER CLIENT      │
│  Shows result       │
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│   YOU (User)        │
│ See container ID    │
│ Container running ✅│
└─────────────────────┘
```

---

## Storage

### How Docker Stores Data 💾

```
Docker uses different storage types:

1) LAYERS
   ├─ Image made of layers
   ├─ Each layer read-only
   ├─ Container adds writable layer on top
   └─ Efficient and fast

2) VOLUMES
   ├─ Persistent data
   ├─ Survive container deletion
   ├─ Shared between containers
   └─ Stored on host

3) BIND MOUNTS
   ├─ Mount host directory
   ├─ Container sees host files
   ├─ Changes sync both ways
   └─ Used for development

4) TMPFS
   ├─ Temporary storage
   ├─ In RAM (super fast)
   ├─ Lost when container stops
   └─ Used for sensitive data
```

### Image Layers 🎂

```
Think of layers like a cake:

┌─────────────────────────┐
│ Container Layer (RW)    │ ◄─ Running changes
│ Your file modifications │   (writable)
├─────────────────────────┤
│ Layer 5 (RO)            │ ◄─ Read-only
│ App config              │   (from image)
├─────────────────────────┤
│ Layer 4 (RO)            │ ◄─ Read-only
│ Node libraries          │
├─────────────────────────┤
│ Layer 3 (RO)            │ ◄─ Read-only
│ Node.js installed       │
├─────────────────────────┤
│ Layer 2 (RO)            │ ◄─ Read-only
│ Debian Linux + packages │
├─────────────────────────┤
│ Layer 1 (RO)            │ ◄─ Read-only
│ Base layer              │   (deepest)
└─────────────────────────┘

When you run container:
├─ All read-only layers stack
├─ Writable layer added on top
├─ You modify files
└─ Original image unchanged!
```

---

## Summary 📝

### Docker Architecture Components:

```
┌─────────────────────────────────────┐
│    DOCKER ARCHITECTURE              │
├─────────────────────────────────────┤
│                                     │
│  CLIENT          DAEMON      DATA   │
│  ┌────────┐     ┌──────┐    ┌───┐  │
│  │ CLI/API│────►│Engine│───►│Img│  │
│  └────────┘     │      │    │Vols│ │
│  User           │Manage│    │Rgs │ │
│  commands       └──────┘    └───┘  │
│                                     │
│  REGISTRY                           │
│  ┌────────────────────┐            │
│  │ Docker Hub         │            │
│  │ Private Registry   │            │
│  │ Cloud Registries   │            │
│  └────────────────────┘            │
│                                     │
└─────────────────────────────────────┘
```

### Key Points:

✅ **Client-Daemon** architecture  
✅ **Client sends commands**, daemon executes  
✅ **Images** are templates (read-only)  
✅ **Containers** are running instances  
✅ **Layers** make images efficient  
✅ **Registries** store images  
✅ **REST API** is underlying protocol  

---

## 🔗 Next Section

👉 **[03-Installation](../03-Installation/README.md)** - Install Docker on your machine

---

## 📚 Key Takeaways

1. **Client-Daemon** = Two-part architecture
2. **Daemon** = Does all the work
3. **Client** = How you give commands
4. **Images** = Read-only templates
5. **Containers** = Running instances
6. **Layers** = Make images efficient
7. **Registry** = Storage for images

---

**Ready to install Docker?** Let's go! 🚀

Last updated: January 2025  
Difficulty: ⭐⭐ Beginner  
Time: 45 minutes
