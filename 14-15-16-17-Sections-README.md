# 14 - Security 🔐 | 15 - Best-Practices ⭐ | 16 - Azure ☁️ | 17 - Kubernetes 🎯

---

## 🔐 14 - Security

### Image Security

```dockerfile
# ✅ SECURE
FROM python:3.9-slim          # Specific version
RUN addgroup -S appuser       # Non-root user
RUN adduser -S appuser -G appuser
COPY --chown=appuser . /app   # Ownership
USER appuser                  # Use non-root
HEALTHCHECK CMD curl localhost:8000

# ❌ INSECURE
FROM ubuntu:latest            # Latest version (unknown)
RUN pip install anything      # No verification
# Runs as root
```

### Best Practices

```
✅ Use specific versions (not latest)
✅ Run as non-root user
✅ Scan images for vulnerabilities
✅ Use minimal base images
✅ Remove build tools from production
✅ Use secrets management
✅ Keep images updated
```

### Scanning Images

```bash
# Scan for vulnerabilities
$ docker scan myimage:1.0

# See vulnerabilities
$ docker scan myimage:1.0 --json

# Fix: Update and rebuild
```

---

## ⭐ 15 - Best-Practices

### General Rules

```
✅ Use specific versions
✅ Minimal base images (alpine)
✅ One process per container
✅ Run as non-root
✅ Clean up after installing
✅ Use .dockerignore
✅ Multi-stage builds
✅ Health checks
✅ Logging
✅ Documentation
```

### Dockerfile Example

```dockerfile
# Bad
FROM ubuntu:latest
RUN apt-get install python
COPY . /app
RUN pip install flask
CMD ["python", "app.py"]

# Good
FROM python:3.9-alpine
WORKDIR /app
RUN addgroup -S app && adduser -S app -G app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY --chown=app . .
USER app
EXPOSE 5000
HEALTHCHECK CMD curl http://localhost:5000
CMD ["python", "app.py"]
```

### docker-compose Best Practices

```yaml
version: '3.8'

services:
  app:
    image: myapp:1.0          # Specific version
    restart: always           # Auto-restart
    environment:              # From .env
      - DEBUG=false
    volumes:
      - app-data:/data        # Named volumes
    healthcheck:              # Monitor health
      test: curl http://localhost:8000
      interval: 30s
    networks:                 # Custom networks
      - app-network

networks:
  app-network:

volumes:
  app-data:
```

---

## ☁️ 16 - Docker with Azure

### Azure Container Instances

```bash
# Run container on Azure
$ az container create \
  --resource-group mygroup \
  --name mycontainer \
  --image myimage:1.0 \
  --ports 80 \
  --environment-variables DEBUG=false

# View logs
$ az container logs --resource-group mygroup --name mycontainer

# Delete
$ az container delete --resource-group mygroup --name mycontainer
```

### Azure Container Registry

```bash
# Create registry
$ az acr create --resource-group mygroup --name myregistry --sku Basic

# Login
$ az acr login --name myregistry

# Tag image
$ docker tag myimage:1.0 myregistry.azurecr.io/myimage:1.0

# Push to Azure
$ docker push myregistry.azurecr.io/myimage:1.0

# Pull from Azure
$ docker pull myregistry.azurecr.io/myimage:1.0

# Run from Azure registry
$ az container create \
  --image myregistry.azurecr.io/myimage:1.0 \
  --registry-login-server myregistry.azurecr.io
```

### Benefits

```
✅ Managed service (no ops)
✅ Auto-scaling
✅ High availability
✅ Integrated with Azure
✅ Easy deployment
```

---

## 🎯 17 - Kubernetes

### What is Kubernetes?

```
Kubernetes = Container orchestration platform
Manages: Multiple hosts, containers, networking, storage
Automates: Deployment, scaling, networking, storage

For: Production, large scale, multi-host
```

### Key Concepts

```
Pod: Smallest unit (one or more containers)
Deployment: Multiple pods
Service: Network access to pods
Namespace: Logical isolation
```

### Running on Kubernetes

```bash
# Create deployment
$ kubectl create deployment myapp --image=myimage:1.0

# Scale deployment
$ kubectl scale deployment myapp --replicas=3

# Expose service
$ kubectl expose deployment myapp --type=LoadBalancer --port=80

# View pods
$ kubectl get pods

# View logs
$ kubectl logs pod-name

# Update image
$ kubectl set image deployment/myapp myapp=myimage:2.0
```

### Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp

spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  
  template:
    metadata:
      labels:
        app: myapp
    
    spec:
      containers:
      - name: myapp
        image: myimage:1.0
        ports:
        - containerPort: 8000
        env:
        - name: DEBUG
          value: "false"
        resources:
          limits:
            memory: "512Mi"
            cpu: "500m"
```

### Why Kubernetes?

```
✅ Auto-scaling
✅ Self-healing
✅ Load balancing
✅ Rolling updates
✅ Resource management
✅ Production ready
❌ Complex to learn
❌ Overkill for small apps
```

### When to Use?

```
Use Docker Compose:
- Development
- Small teams
- Simple applications

Use Kubernetes:
- Production
- Large scale
- Multiple servers
- High availability needed
```

---

## Summary

```
14-Security: Keep apps secure
15-Best-Practices: Follow industry standards
16-Azure: Deploy to cloud
17-Kubernetes: Enterprise orchestration

All production-ready!
```

---

Last updated: January 2025
Difficulty: ⭐⭐⭐ Advanced
