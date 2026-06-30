# 04 - Docker Images 🖼️

**Understanding Images - The Blueprint of Containers**

---

## 📚 Table of Contents

1. [What is an Image?](#what-is-an-image)
2. [Image vs Container](#image-vs-container)
3. [Image Layers](#image-layers)
4. [Pulling Images](#pulling-images)
5. [Listing Images](#listing-images)
6. [Image Commands](#image-commands)
7. [Public Images](#public-images)
8. [Summary](#summary)

---

## What is an Image?

### Simple Definition 🎯

**An image is a lightweight, standalone, executable blueprint that contains:**

```
✅ Operating System (minimal Linux)
✅ Your application code
✅ All libraries and dependencies
✅ Environment variables
✅ Configuration files
✅ Instructions to run your app
```

### Think of it Like... 📦

```
IMAGE = FROZEN FOOD BOX
├─ Contains all ingredients
├─ Has instructions
├─ Ready to cook
├─ Reusable (make many meals)
└─ Shareable with others

CONTAINER = COOKED MEAL
├─ Actual running food
├─ Created from frozen box
├─ Ready to eat
└─ Temporary (eat and done)
```

### Visual Representation 🎨

```
IMAGE (Read-only blueprint)
┌──────────────────────────┐
│ FROM python:3.9          │
│ ├─ Python 3.9            │
│ ├─ Linux OS (minimal)    │
│ ├─ System libraries      │
│ │                        │
│ COPY app.py /app/        │
│ ├─ Your code             │
│ │                        │
│ RUN pip install flask    │
│ ├─ Flask library         │
│ ├─ Dependencies          │
│ │                        │
│ CMD python app.py        │
│ ├─ Startup command       │
│                          │
│ Size: ~150 MB            │
│ Status: Template/Stored  │
└──────────────────────────┘
        ↓ docker run
     ┌──────────────────────┐
     │ CONTAINER (Running)  │
     ├──────────────────────┤
     │ PID: 1234            │
     │ Memory: 50 MB        │
     │ Status: Up 2 minutes │
     │ Port: 8000:5000      │
     └──────────────────────┘
```

---

## Image vs Container

### Key Differences 🔑

| Aspect | Image | Container |
|--------|-------|-----------|
| **What is it?** | Template/Blueprint | Running instance |
| **State** | Static (doesn't change) | Dynamic (changing) |
| **Stored** | On disk | In memory |
| **Writable** | No (read-only) | Yes (writable layer) |
| **Multiple copies** | One image | Many containers |
| **Size** | Smaller (~100MB) | Larger (with data) |
| **Lifecycle** | Created once, used many | Created/destroyed often |

### Relationship 🔗

```
         IMAGE
    (Template/Blueprint)
           │
           ├──────────────────────┐
           │                      │
        docker run            docker run
           │                      │
           ▼                      ▼
       CONTAINER 1           CONTAINER 2
      (Running App 1)        (Running App 2)
      
Same image = Many containers!

Like:
┌─ One recipe → Many dishes
┌─ One blueprint → Many houses
┌─ One song → Many performances
```

### Visual Analogy 📊

```
BAKERY EXAMPLE:

IMAGE = RECIPE
├─ Ingredients list
├─ Instructions
├─ Measurements
└─ Fixed, reusable

CONTAINER = BAKED CAKE
├─ Actual baked cake
├─ Has taste, texture
├─ Can eat it
├─ Temporary (gets eaten)

Many cakes from one recipe! 🎂🎂🎂
Many containers from one image! 🐳🐳🐳
```

---

## Image Layers

### Understanding Layers 🎂

Docker images are built in **layers**, like a cake:

```
LAYER VISUALIZATION:

┌─────────────────────────────────────┐
│ Layer 5: Your app modifications    │  ◄─ TOP
│ (Commands in Dockerfile)            │
├─────────────────────────────────────┤
│ Layer 4: Your application code      │
│ (COPY/ADD commands)                 │
├─────────────────────────────────────┤
│ Layer 3: Dependencies installed     │
│ (RUN pip install)                   │
├─────────────────────────────────────┤
│ Layer 2: Base OS and tools          │
│ (FROM python:3.9)                   │
├─────────────────────────────────────┤
│ Layer 1: Base scratch layer         │
│ (Empty foundation)                  │
└─────────────────────────────────────┘
      ↓ (All read-only in image)
      
CONTAINER uses image:
┌─────────────────────────────────────┐
│ Writable Layer (Container layer)    │ ◄─ RUNNING
│ (Your changes here)                 │
├─────────────────────────────────────┤
│ Layer 5 (Read-only from image)      │
├─────────────────────────────────────┤
│ Layer 4 (Read-only from image)      │
├─────────────────────────────────────┤
│ Layer 3 (Read-only from image)      │
├─────────────────────────────────────┤
│ Layer 2 (Read-only from image)      │
├─────────────────────────────────────┤
│ Layer 1 (Read-only from image)      │
└─────────────────────────────────────┘
```

### Why Layers? 🤔

```
Benefits:
1) EFFICIENCY
   ├─ Only changed layers rebuild
   ├─ Same layers shared between images
   └─ Save disk space

2) REUSABILITY
   ├─ Layers can be reused
   ├─ Like sharing Lego blocks
   └─ Build faster

3) CACHING
   ├─ Docker caches layers
   ├─ Rebuilding is faster
   └─ Only new changes process

Example:
First build: 10 minutes
Second build (same layers): 2 seconds!
```

### Layer Example 📦

```
Dockerfile:
┌────────────────────────────┐
│ FROM python:3.9            │ ─► Layer 1
│ WORKDIR /app               │ ─► Layer 2
│ COPY requirements.txt .    │ ─► Layer 3
│ RUN pip install -r req.txt │ ─► Layer 4
│ COPY . .                   │ ─► Layer 5
│ CMD python app.py          │ ─► Layer 6
└────────────────────────────┘

$ docker build .
  Step 1 : FROM... ✅ Layer 1 created (cache hit)
  Step 2 : WORKDIR... ✅ Layer 2 created (cache hit)
  Step 3 : COPY... ✅ Layer 3 created (new layer)
  Step 4 : RUN... ✅ Layer 4 created (new layer)
  Step 5 : COPY... ✅ Layer 5 created (new layer)
  Step 6 : CMD... ✅ Layer 6 created (meta info)
  
Result: Image with 6 layers ✅
```

---

## Pulling Images

### What is Pulling? 📥

**Pulling = Downloading an image from Docker Hub to your computer**

```
Think of it like:
├─ GitHub: Clone a repo
├─ Spotify: Download a song
├─ Netflix: Stream (download) a movie
└─ Docker: Pull an image

Before pulling:
$ docker images
REPOSITORY    TAG    IMAGE ID    SIZE
(no images)

After pulling:
$ docker pull python:3.9
3.9: Pulling from library/python
...downloading layers...
Done! Image downloaded!

Now you can use it!
$ docker run python:3.9 python --version
Python 3.9.x
```

### How to Pull 🔍

```bash
# Basic syntax
$ docker pull image-name:tag

Examples:

# Pull latest Python
$ docker pull python

# Pull specific version
$ docker pull python:3.9

# Pull from specific registry
$ docker pull myregistry.com/myimage:v1

# Pull Ubuntu
$ docker pull ubuntu:22.04

# Pull Nginx
$ docker pull nginx:latest
```

### Understanding Tags 🏷️

```
Image name format:
repository/image:tag

Example: python:3.9
├─ python = image name
└─ 3.9 = tag (version)

Common tags:
├─ latest = Most recent version
├─ 3.9 = Specific version
├─ 3.9-alpine = Version + variant
├─ bullseye = Based on Debian bullseye
└─ slim = Minimal version
```

---

## Listing Images

### View Your Images 📋

```bash
# List all images
$ docker images

Output:
REPOSITORY      TAG      IMAGE ID      SIZE
python          3.9      8a7c6f8b1e2   980MB
nginx           latest   2c50e21b4d8   142MB
ubuntu          22.04    ba6acccedd29  77.8MB
hello-world     latest   feb5d9fea6a   13.3kB

# List with more info
$ docker images -a

# Search for specific image
$ docker images | grep python
python          3.9      8a7c6f8b1e2   980MB

# List formatted
$ docker images --format "table {{.Repository}}\t{{.Size}}"
```

### Reading Image Details 🔎

```
REPOSITORY:
├─ Name of the image
└─ Example: python, nginx, ubuntu

TAG:
├─ Version identifier
├─ latest = newest version
└─ Example: 3.9, 22.04, latest

IMAGE ID:
├─ Unique identifier
├─ First 12 characters shown
└─ Used to identify images

SIZE:
├─ How much disk space
├─ Includes all layers
└─ python:3.9 = ~980 MB
```

---

## Image Commands

### Essential Image Commands 🔧

```bash
# PULL - Download image
$ docker pull python:3.9

# IMAGES - List images
$ docker images

# INSPECT - Get image details
$ docker inspect python:3.9

# REMOVE - Delete image
$ docker rmi image-name
$ docker rmi python:3.9

# TAG - Create alias for image
$ docker tag python:3.9 my-python:1.0

# PUSH - Upload image to registry
$ docker push username/image-name

# BUILD - Create image from Dockerfile
$ docker build -t myimage:1.0 .

# HISTORY - View image layers
$ docker history image-name

# PRUNE - Delete unused images
$ docker image prune

# SEARCH - Search Docker Hub
$ docker search python
```

### Common Use Cases 💡

```bash
# Get latest Nginx
$ docker pull nginx

# Check if image exists
$ docker images | grep python

# Remove image
$ docker rmi python:3.9

# Make a copy with different name
$ docker tag python:3.9 mypy:latest

# See how image is built
$ docker history python:3.9

# Find all unused images
$ docker images -f "dangling=true"

# Remove all unused images
$ docker image prune -a
```

---

## Public Images

### Docker Hub 🐳

**Docker Hub** is like GitHub for Docker images:

```
Website: https://hub.docker.com
├─ Search images
├─ Share your images
├─ Download public images
├─ Create private repositories
└─ Millions of images available

Features:
├─ Official images (maintained by companies)
├─ Community images (users upload)
├─ Automated builds
├─ Version history
└─ Download statistics
```

### Popular Official Images 🌟

```
Most downloaded images:

1. library/ubuntu
   ├─ Linux OS
   └─ 10 billion+ pulls

2. library/nginx
   ├─ Web server
   └─ 5 billion+ pulls

3. library/mysql
   ├─ Database
   └─ 2 billion+ pulls

4. library/python
   ├─ Programming language
   └─ 1 billion+ pulls

5. library/redis
   ├─ Cache/database
   └─ 1 billion+ pulls
```

### How to Find Images 🔍

```bash
# Search Docker Hub from terminal
$ docker search python

Output:
NAME                DESCRIPTION          STARS   OFFICIAL
python              Python is a high-...  4500    [OK]
pypy                PyPy is a fast...     400
jython              Jython is a...        50

# Or browse online
https://hub.docker.com/search?q=python

# Find official images only
$ docker search --is-official python
```

---

## Summary 📝

```
KEY CONCEPTS:

✅ Image = Blueprint/Template
✅ Container = Running instance
✅ Layers = Stacked components
✅ Pull = Download image
✅ Tag = Version label
✅ Registry = Storage (Docker Hub)

WORKFLOW:
1. Pull image: docker pull python:3.9
2. List images: docker images
3. Run container: docker run python:3.9
4. Use container: Your app runs
5. Stop container: docker stop
6. Remove image: docker rmi
```

---

## 🔗 Next Section

👉 **[05-Containers](../05-Containers/README.md)** - Create and Manage Containers

---

## 📚 Key Takeaways

1. **Image** = Read-only template
2. **Container** = Running instance
3. **Layers** = Efficient stacking
4. **Pull** = Download from registry
5. **Docker Hub** = Central repository
6. **Tags** = Version identifiers

---

**Ready to run containers?** Let's go! 🚀

Last updated: January 2025  
Difficulty: ⭐⭐ Beginner  
Time: 1 hour
