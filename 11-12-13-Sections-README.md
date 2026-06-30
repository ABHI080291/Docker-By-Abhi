# 11 - Docker Hub 📦 | 12 - Private Registry 🔒 | 13 - Multi-Stage Build 🏗️

---

## 📦 11 - Docker Hub

### What is Docker Hub?

```
Docker Hub = Central repository for Docker images
Like: GitHub for images
```

### Using Docker Hub

```bash
# Login
$ docker login

# Push image
$ docker tag myimage:1.0 username/myimage:1.0
$ docker push username/myimage:1.0

# Pull image
$ docker pull username/myimage:1.0

# Create repository on hub.docker.com first!
```

### Public vs Private

```
Public repositories:
✅ Free
✅ Anyone can pull
✅ Share with community

Private repositories:
🔒 Paid
🔒 Only authorized users
🔒 For companies
```

---

## 🔒 12 - Private Registry

### Self-Hosted Registry

```bash
# Run private registry
$ docker run -d -p 5000:5000 registry:2

# Tag for private registry
$ docker tag myimage:1.0 localhost:5000/myimage:1.0

# Push to private
$ docker push localhost:5000/myimage:1.0

# Pull from private
$ docker pull localhost:5000/myimage:1.0
```

### Docker Compose

```yaml
version: '3.8'

services:
  registry:
    image: registry:2
    ports:
      - "5000:5000"
    volumes:
      - registry-data:/var/lib/registry

volumes:
  registry-data:
```

### Cloud Registries

```
AWS ECR (Elastic Container Registry)
Google GCR (Google Container Registry)
Azure ACR (Azure Container Registry)
All offer:
✅ Private hosting
✅ High availability
✅ Security
✅ Integration with cloud services
```

---

## 🏗️ 13 - Multi-Stage Build

### Why Multi-Stage?

```
Problem:
Build container = 1 GB (has build tools)
Runtime container = 1 GB
Total = 2 GB ❌ HUGE!

Solution:
Build in one stage, use in another
Final = 100 MB ✅ SMALL!
```

### Build vs Runtime

```dockerfile
# Stage 1: BUILD (heavy)
FROM node:16
WORKDIR /app
COPY . .
RUN npm install
RUN npm run build

# Stage 2: RUNTIME (lightweight)
FROM node:16-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
RUN npm install --production
CMD ["node", "dist/index.js"]

Result:
✅ Only dist folder in final image
✅ No build tools in production
✅ Smaller = Faster = Cheaper
```

### Real Example

```dockerfile
# Builder stage
FROM golang:1.19 as builder
WORKDIR /app
COPY . .
RUN CGO_ENABLED=0 go build -o app

# Runtime stage  
FROM alpine:latest
RUN apk add --no-cache ca-certificates
WORKDIR /root/
COPY --from=builder /app/app .
CMD ["./app"]

Image size: 10 MB (vs 800 MB without multi-stage!)
```

### Multi-Stage Benefits

```
✅ Smaller images
✅ Faster deployment
✅ Less disk space
✅ No build tools in production
✅ Better security
✅ Cheaper to run
```

---

## Summary

```
Hub: Share images publicly
Registry: Private self-hosted storage
Multi-Stage: Optimize image size

All three important for:
✅ Production deployment
✅ Team collaboration
✅ Image optimization
```

---

Last updated: January 2025
Difficulty: ⭐⭐ Intermediate
