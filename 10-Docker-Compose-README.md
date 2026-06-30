# 10 - Docker Compose 🎼

**Run Multiple Containers Together - Easy Orchestration!**

---

## 📚 Table of Contents

1. [What is Docker Compose?](#what-is-docker-compose)
2. [Installation](#installation)
3. [docker-compose.yml](#docker-composeyml)
4. [Services](#services)
5. [Volumes](#volumes)
6. [Networking](#networking)
7. [Environment Variables](#environment-variables)
8. [Running with Compose](#running-with-compose)
9. [Real Examples](#real-examples)
10. [Summary](#summary)

---

## What is Docker Compose?

### Simple Definition 🎯

**Docker Compose is a tool that lets you run multiple containers together with one command**

```
Before Docker Compose:
├─ Start web server: docker run nginx
├─ Start database: docker run mysql
├─ Start cache: docker run redis
├─ Setup networking: docker network create
├─ Connect them: complex setup
└─ Result: Complicated! 😫

With Docker Compose:
├─ Write services in docker-compose.yml
├─ Run: docker-compose up
├─ Everything starts together! 
└─ Result: Simple! 😊
```

### Think of it Like... 🎼

```
Docker Compose = Orchestra conductor
├─ Docker = Individual musicians
├─ Compose = Conductor's sheet music
└─ Result = Perfect harmony! 🎵

Dockerfile = Single recipe
Docker Compose = Multiple recipes coordinated together
```

### Analogy 🍕

```
Without Compose (Making pizza alone):
├─ Step 1: Prepare dough
├─ Step 2: Make sauce
├─ Step 3: Prepare toppings
├─ Step 4: Combine all
├─ Step 5: Bake
└─ Result: One pizza, many steps, no coordination

With Compose (Restaurant):
├─ All stations work together
├─ Dough station, sauce station, toppings, oven
├─ All coordinated
├─ Result: Many pizzas, perfect coordination! 🍕
```

### When to Use Compose ⚙️

Use Docker Compose when you have:

✅ Multiple services (web, database, cache)  
✅ Need them to work together  
✅ Want easy start/stop  
✅ Development environment setup  
✅ Testing environment setup  
✅ Multi-container applications  

---

## Installation

### Check if Installed 🔍

```bash
# Check Docker Compose version
$ docker-compose --version

# Should show:
Docker Compose version 2.15.1, build xyz

# If not installed, see below
```

### Install Docker Compose 📥

**For Windows/Mac:**
```
Docker Desktop already includes Compose!
✅ No additional installation needed
✅ Comes with Docker Desktop
✅ Ready to use!
```

**For Linux:**
```bash
# Install with curl
$ sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# Make executable
$ sudo chmod +x /usr/local/bin/docker-compose

# Verify
$ docker-compose --version
```

---

## docker-compose.yml

### File Structure 📝

```yaml
version: '3.8'              # Docker Compose version

services:                   # Services (containers)
  web:                      # Service name
    image: nginx:latest     # Image to use
    ports:
      - "8080:80"           # Port mapping
    volumes:
      - ./html:/usr/share/nginx/html  # Mount folder
    environment:
      - ENV=production
    
  database:                 # Another service
    image: mysql:8.0
    ports:
      - "3306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: secretpass
    volumes:
      - db_data:/var/lib/mysql  # Named volume

  cache:                    # Third service
    image: redis:latest
    ports:
      - "6379:6379"

volumes:                    # Defined volumes
  db_data:                  # Volume name

networks:                   # Custom networks (optional)
  mynetwork:
    driver: bridge
```

### Understanding the File 🔍

```
version: '3.8'
└─ Docker Compose format version
   (backward compatible)

services:
├─ All containers you want to run
├─ Each service = one container
└─ Services can communicate

web: (service name)
├─ Can be any name
├─ Use this to refer to the service
└─ Used for networking

image: nginx:latest
├─ Which image to use
├─ Just like docker run

ports: 8080:80
├─ Map port 8080→80
├─ Access nginx on http://localhost:8080

volumes:
├─ Persistent data
├─ Mount folders
└─ Share data between services

environment:
├─ Environment variables
├─ Configuration values
└─ Passed to container
```

---

## Services

### What is a Service? 🎯

```
Service = Configuration for a container

In docker-compose.yml:
web:                      ← Service definition
  image: nginx:latest     ← What to run
  ports:
    - "8080:80"           ← Port mapping
  volumes:
    - ./code:/app          ← Volume
  environment:            ← Variables
    - DEBUG=true

When you run: docker-compose up
Docker creates and starts container
Based on this configuration!
```

### Service Options 🛠️

```yaml
services:
  myapp:
    image: myimage:1.0              # Image to use
    build: ./dockerfile             # Or build from Dockerfile
    container_name: my-container    # Container name
    
    ports:
      - "8080:80"                   # Port mapping
      - "443:443"
    
    volumes:
      - ./code:/app                 # Bind mount
      - data:/data                  # Named volume
    
    environment:
      - KEY=value                   # Environment vars
      - DEBUG=true
    
    env_file: .env                  # From file
    
    command: python app.py          # Override CMD
    
    depends_on:
      - database                    # Wait for other service
    
    networks:
      - mynetwork                   # Custom network
    
    restart: always                 # Auto-restart policy
    
    healthcheck:                    # Health checks
      test: curl localhost
      interval: 30s
    
    links:
      - database                    # Legacy networking
    
    cpu_shares: 512                 # CPU limit
    mem_limit: 512mb                # Memory limit
```

### Multiple Services 🐳🐳🐳

```yaml
version: '3.8'

services:
  # Web service
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    depends_on:
      - api
  
  # API service
  api:
    build: ./api
    ports:
      - "5000:5000"
    depends_on:
      - database
  
  # Database service
  database:
    image: postgres:13
    ports:
      - "5432:5432"
    environment:
      - POSTGRES_PASSWORD=secret
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

---

## Volumes

### Types of Volumes in Compose 💾

```
1) Bind Mounts (Host folder)
   volumes:
     - ./code:/app
   └─ Host folder → Container folder

2) Named Volumes (Managed by Docker)
   volumes:
     mydata:/data
   └─ Docker-managed volume

3) Anonymous Volumes
   volumes:
     - /data
   └─ Temporary volume
```

### Volume Examples 📦

```yaml
services:
  web:
    image: nginx:latest
    volumes:
      # Bind mount (host:container)
      - ./html:/usr/share/nginx/html
      - ./config:/etc/nginx/conf.d
      
      # Named volume
      - web_data:/data
      
      # Read-only
      - ./config:/etc/nginx:ro

  database:
    image: mysql:8.0
    volumes:
      # Persist database data
      - db_data:/var/lib/mysql

volumes:
  # Define named volumes
  web_data:
  db_data:
```

### Persistent Data ✅

```yaml
version: '3.8'

services:
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: secret
    volumes:
      # Data persists even if container deleted!
      - mysql_data:/var/lib/mysql

volumes:
  mysql_data:

# Now:
# 1. $ docker-compose up
#    Container starts, creates database
#
# 2. $ docker-compose down
#    Container stops BUT data remains!
#
# 3. $ docker-compose up
#    Container starts with same data!
```

---

## Networking

### Automatic Networking 🌐

```
Docker Compose creates a network automatically!

services:
  web:
    image: nginx
  database:
    image: mysql

Result:
└─ Both on same network
└─ Can communicate
└─ Use service name as hostname!

Example:
  web container can access database as:
  ├─ hostname: database
  ├─ URL: http://database:3306
  └─ Or: mysql://database:3306
```

### Service Discovery 🔍

```yaml
version: '3.8'

services:
  web:
    image: nginx
    # Can access:
    # - http://api:5000
    # - http://database:5432
  
  api:
    image: myapi
    # Can access:
    # - http://web:80
    # - http://database:5432
  
  database:
    image: postgres
    # Can access:
    # - http://web:80
    # - http://api:5000
```

### Custom Networks 🎯

```yaml
version: '3.8'

services:
  web:
    image: nginx
    networks:
      - frontend

  api:
    image: myapi
    networks:
      - frontend
      - backend

  database:
    image: postgres
    networks:
      - backend

networks:
  frontend:
    driver: bridge
  backend:
    driver: bridge

# Result:
# frontend: web ← → api
# backend: api ← → database
# web cannot talk to database directly!
```

---

## Environment Variables

### Setting Variables 🔧

```yaml
services:
  database:
    image: mysql:8.0
    environment:
      # Direct values
      MYSQL_ROOT_PASSWORD: secret123
      MYSQL_DATABASE: myapp
      MYSQL_USER: appuser
      MYSQL_PASSWORD: apppass

  web:
    image: myapp
    environment:
      # Multiple ways
      - DEBUG=true
      - LOG_LEVEL=info
      - DB_HOST=database
      - DB_USER=appuser
      - DB_PASSWORD=apppass
```

### From .env File 📄

```yaml
# docker-compose.yml
services:
  database:
    image: mysql:8.0
    env_file:
      - .env
    environment:
      MYSQL_ROOT_PASSWORD: ${DB_PASSWORD}
      MYSQL_DATABASE: ${DB_NAME}

# .env file
DB_PASSWORD=secret123
DB_NAME=myapp
DB_USER=admin
DEBUG=true
```

### Using Variables 💡

```yaml
# docker-compose.yml
version: '3.8'

services:
  database:
    image: mysql:8.0
    ports:
      - "${DB_PORT}:3306"
    environment:
      MYSQL_ROOT_PASSWORD: ${DB_PASSWORD}
      MYSQL_DATABASE: ${DB_NAME}

  web:
    image: nginx:${NGINX_VERSION}
    ports:
      - "${WEB_PORT}:80"
    environment:
      - DATABASE_URL=mysql://root:${DB_PASSWORD}@database:3306/${DB_NAME}

# .env file
DB_PORT=3306
DB_PASSWORD=secret
DB_NAME=myapp
NGINX_VERSION=latest
WEB_PORT=8080
```

---

## Running with Compose

### Basic Commands ▶️

```bash
# Start all services (create + start)
$ docker-compose up

# Start in background
$ docker-compose up -d

# Stop all services
$ docker-compose down

# View running services
$ docker-compose ps

# View logs
$ docker-compose logs

# View specific service logs
$ docker-compose logs -f database

# Restart services
$ docker-compose restart

# Build images from Dockerfile
$ docker-compose build

# Build and start
$ docker-compose up --build
```

### Full Workflow 🔄

```bash
# 1. Create docker-compose.yml
#    (with services definition)

# 2. Start everything
$ docker-compose up -d

# 3. Check status
$ docker-compose ps

# 4. View logs
$ docker-compose logs

# 5. Access services
#    Web: http://localhost:8080
#    API: http://localhost:5000

# 6. Make changes
#    Edit docker-compose.yml

# 7. Update
$ docker-compose up -d

# 8. Stop
$ docker-compose down

# 9. Cleanup (remove volumes)
$ docker-compose down -v
```

---

## Real Examples

### Example 1: Web + Database 🌐📊

```yaml
# docker-compose.yml
version: '3.8'

services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
    networks:
      - mynet

  database:
    image: mysql:8.0
    ports:
      - "3306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: myapp
    volumes:
      - mysql_data:/var/lib/mysql
    networks:
      - mynet

volumes:
  mysql_data:

networks:
  mynet:
```

**Usage:**
```bash
$ docker-compose up -d
$ # Visit http://localhost:8080
$ docker-compose ps
$ docker-compose down
```

### Example 2: Full Stack App 🏗️

```yaml
version: '3.8'

services:
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    depends_on:
      - backend

  backend:
    build: ./backend
    ports:
      - "5000:5000"
    environment:
      DATABASE_URL: postgresql://user:pass@postgres:5432/db
    depends_on:
      - postgres

  postgres:
    image: postgres:13
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: db
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

---

## Summary 📝

```
KEY CONCEPTS:

✅ Compose = Multi-container orchestration
✅ docker-compose.yml = Configuration file
✅ Services = Containers to run
✅ Volumes = Persistent data
✅ Networks = Container communication
✅ Environment = Configuration variables

WORKFLOW:
1. Write docker-compose.yml
2. Run: docker-compose up -d
3. Check: docker-compose ps
4. View logs: docker-compose logs
5. Stop: docker-compose down
```

---

## 🔗 Next Section

👉 **[11-Docker-Hub](../11-Docker-Hub/README.md)** - Share Your Images

---

## 📚 Key Takeaways

1. **Compose** = Run multiple containers
2. **docker-compose.yml** = Configuration file
3. **Services** = Each container definition
4. **Volumes** = Persistent data
5. **Networks** = Container communication
6. **Environment** = Configuration values

---

**Ready to share your images?** Let's go! 🚀

Last updated: January 2025  
Difficulty: ⭐⭐ Intermediate  
Time: 2 hours
