# 09 - Networking 🌐

**Connect Containers Together**

---

## What is Container Networking?

### Simple Definition 🎯

**Container networking allows containers to communicate with each other and external systems**

```
Without networking:
Containers isolated ❌ Can't talk to each other

With networking:
Containers connected ✅ Can communicate freely
```

### Network Types

```
1) Bridge (default)
   └─ Containers on host can communicate

2) Host
   └─ Container uses host network directly

3) Overlay
   └─ Multi-host communication

4) Macvlan
   └─ Assign MAC addresses

5) None
   └─ No network
```

---

## Creating Networks

### Create Bridge Network

```bash
# Create network
$ docker network create mynetwork

# List networks
$ docker network ls

# Inspect network
$ docker network inspect mynetwork

# Remove network
$ docker network rm mynetwork
```

### Run Container on Network

```bash
# Run on custom network
$ docker run --network mynetwork --name db mysql:8.0

# Connect existing container
$ docker network connect mynetwork container-id

# Disconnect
$ docker network disconnect mynetwork container-id
```

---

## Container Communication

### Service Discovery

```
Containers on same network can communicate using:
- Service name as hostname
- Example: http://database:3306

No IP addresses needed!
```

### Real Example

```bash
# Create network
$ docker network create app-network

# Run database
$ docker run -d --network app-network --name db mysql:8.0 \
  -e MYSQL_ROOT_PASSWORD=secret

# Run app (can reach db via "db" hostname)
$ docker run -d --network app-network --name app myapp:1.0

# Inside app container:
# Can access database as: mysql://root:secret@db:3306/database
```

---

## Docker Compose Networking

### Automatic Networking

```yaml
version: '3.8'

services:
  web:
    image: nginx
    # Automatically on network!
    # Can access: http://database:3306
  
  database:
    image: mysql:8.0
    # Both services connected!
```

### Custom Networks

```yaml
version: '3.8'

services:
  frontend:
    image: nginx
    networks:
      - frontend-network
  
  backend:
    image: myapp:1.0
    networks:
      - backend-network
      - frontend-network
  
  database:
    image: mysql:8.0
    networks:
      - backend-network

networks:
  frontend-network:
    driver: bridge
  backend-network:
    driver: bridge
```

---

## Port Mapping

### Expose Ports

```bash
# Map single port
$ docker run -p 8080:80 nginx

# Map multiple
$ docker run -p 8080:80 -p 8443:443 nginx

# Specify IP
$ docker run -p 127.0.0.1:8080:80 nginx
```

### In docker-compose.yml

```yaml
services:
  web:
    image: nginx
    ports:
      - "8080:80"        # HTTP
      - "8443:443"       # HTTPS
```

---

## Real Example: Web + Database

```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "8080:5000"
    depends_on:
      - db
    environment:
      # Can reach database as "db"
      - DATABASE_URL=mysql://user:pass@db:3306/myapp
    networks:
      - app-network

  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: myapp
    networks:
      - app-network

networks:
  app-network:
```

---

## Summary

```
✅ Networks connect containers
✅ Automatic service discovery
✅ Port mapping for external access
✅ Multiple network types
✅ Docker Compose handles automatically
```

---

**Master container networking!** 🌐

Last updated: January 2025
Difficulty: ⭐⭐ Beginner
