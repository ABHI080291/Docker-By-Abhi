# 08 - Volumes 💾

**Persist Data in Docker Containers**

---

## 📚 Table of Contents

1. [What are Volumes?](#what-are-volumes)
2. [Volume Types](#volume-types)
3. [Creating Volumes](#creating-volumes)
4. [Using Volumes](#using-volumes)
5. [Persistent Data](#persistent-data)
6. [Real Examples](#real-examples)
7. [Summary](#summary)

---

## What are Volumes?

### Simple Definition 🎯

**Volumes are folders where data persists even after container stops or is deleted**

```
Without Volume:
Container deleted → Data lost forever! ❌

With Volume:
Container deleted → Data still exists! ✅
```

### Why Volumes? 💡

```
Problems Without Volumes:
❌ Database data lost when container stops
❌ Files created in container disappear
❌ No data sharing between containers
❌ Can't backup data

Solutions With Volumes:
✅ Data persists
✅ Share data between containers
✅ Easy backup
✅ Separation of data and app
```

### Visual Explanation 📊

```
WITHOUT VOLUME:
┌──────────────────┐
│   CONTAINER      │
│ ├─ App code      │
│ ├─ Database      │
│ └─ Files         │
└──────────────────┘
        │
    Container deleted
        │
        ▼
    ❌ ALL DATA LOST!

WITH VOLUME:
┌──────────────────┐           ┌──────────────┐
│   CONTAINER      │──mount──→ │   VOLUME     │
│ ├─ App code      │           │ (on host)    │
│ ├─ References DB │           │ Database     │
│ └─ Files         │           │ Files        │
└──────────────────┘           └──────────────┘
        │
    Container deleted
        │
        ▼
    ✅ DATA STILL EXISTS IN VOLUME!
```

---

## Volume Types

### 1️⃣ Named Volumes (Docker-Managed)

```
What: Docker manages the volume
Where: /var/lib/docker/volumes/
Use: Databases, persistent data

Benefits:
✅ Easy to manage
✅ Docker handles everything
✅ Can share between containers
✅ Easy backup

Command:
$ docker volume create mydata
$ docker run -v mydata:/data image
```

### 2️⃣ Bind Mounts (Host Folder)

```
What: Mount host folder to container
Where: Any folder on your computer
Use: Development, editing files

Benefits:
✅ Full control
✅ Can edit files on host
✅ Two-way sync
✅ Good for development

Command:
$ docker run -v /host/path:/container/path image
```

### 3️⃣ tmpfs (Temporary)

```
What: In-memory storage
Where: RAM
Use: Temporary data, secrets

Benefits:
✅ Super fast
✅ Private to container
✅ Lost on restart

Command:
$ docker run --tmpfs /temp image
```

### Comparison Table

| Type | Managed | Location | Use Case |
|------|---------|----------|----------|
| Named | ✅ Docker | Host drive | Databases |
| Bind | ❌ Manual | Host folder | Development |
| tmpfs | ❌ No | RAM | Temporary |

---

## Creating Volumes

### Create Volume

```bash
# Create named volume
$ docker volume create mydata

# Create with driver
$ docker volume create -d local mydata

# List volumes
$ docker volume ls

# Inspect volume
$ docker volume inspect mydata

# Remove volume
$ docker volume rm mydata

# Remove unused
$ docker volume prune
```

### Volume Details

```bash
# See where volume is stored
$ docker volume inspect mydata
[
  {
    "Name": "mydata",
    "Driver": "local",
    "Mountpoint": "/var/lib/docker/volumes/mydata/_data"
  }
]
```

---

## Using Volumes

### Run Container with Volume

```bash
# Named volume
$ docker run -v mydata:/data image

# Bind mount
$ docker run -v /host/path:/container/path image

# Read-only
$ docker run -v mydata:/data:ro image

# Multiple volumes
$ docker run -v vol1:/data1 -v vol2:/data2 image
```

### Docker Compose with Volumes

```yaml
version: '3.8'

services:
  db:
    image: mysql:8.0
    volumes:
      # Named volume
      - mysql_data:/var/lib/mysql
    
  web:
    image: nginx
    volumes:
      # Bind mount
      - ./html:/usr/share/nginx/html
      # Named volume
      - shared:/app/data

volumes:
  mysql_data:
  shared:
```

---

## Persistent Data

### Database Example 💾

```yaml
# docker-compose.yml
version: '3.8'

services:
  database:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: myapp
    volumes:
      # Database data persists!
      - db_data:/var/lib/mysql
    ports:
      - "3306:3306"

volumes:
  db_data:
```

### What Happens

```bash
# First run - creates database
$ docker-compose up -d

# Create some data in database
$ docker exec database mysql -u root -p myapp -e "INSERT INTO users VALUES (1, 'John');"

# Stop containers
$ docker-compose down

# Data still in volume! ✅
$ docker volume ls
db_data

# Start again
$ docker-compose up -d

# Data is still there! ✅
$ docker exec database mysql -u root -p myapp -e "SELECT * FROM users;"
+----+------+
| id | name |
+----+------+
| 1  | John |
+----+------+
```

---

## Real Examples

### Example 1: PostgreSQL Database

```yaml
version: '3.8'

services:
  postgres:
    image: postgres:13
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: myapp
    volumes:
      # Persist database
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

volumes:
  postgres_data:
```

### Example 2: Development Setup

```yaml
version: '3.8'

services:
  app:
    image: python:3.9
    working_dir: /app
    volumes:
      # Edit code on host, see changes in container
      - ./code:/app
      # Persistent cache
      - pip_cache:/root/.cache/pip
    command: python app.py

volumes:
  pip_cache:
```

### Example 3: Logs Persistence

```bash
# Create app container
$ docker run -d \
  -v app_logs:/app/logs \
  --name myapp \
  myimage:latest

# Logs persist even after container stops
$ docker logs myapp
$ docker stop myapp
$ docker rm myapp

# Logs still available!
$ docker volume ls
$ docker run -v app_logs:/logs busybox cat /logs/app.log
```

---

## Summary 📝

```
KEY CONCEPTS:

✅ Volumes = Persistent data storage
✅ Named volumes = Docker-managed
✅ Bind mounts = Host folder
✅ tmpfs = In-memory
✅ Persists = Survives container deletion

WORKFLOW:
1. Create volume: docker volume create
2. Mount to container: -v volume:/path
3. Add data inside container
4. Container stops/deletes
5. Data remains in volume!
```

---

## 🔗 Next Section

👉 **[09-Networking](../09-Networking/README.md)** - Connect Containers

---

**Master data persistence with volumes!** 💾

Last updated: January 2025  
Difficulty: ⭐⭐ Beginner
