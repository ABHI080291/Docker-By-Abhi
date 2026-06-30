# 🎤 Docker Interview Questions

**Prepare for Your Docker Job Interview**

---

## 📚 Table of Contents

1. [Beginner Level](#beginner-level)
2. [Intermediate Level](#intermediate-level)
3. [Advanced Level](#advanced-level)
4. [Company-Specific Questions](#company-specific-questions)
5. [Tips for Interviews](#tips-for-interviews)

---

## Beginner Level

### 1. What is Docker?

**Expected Answer:**
```
Docker is a containerization platform that allows you to:
- Package applications with all dependencies
- Run same application on any machine
- Ensure consistency across environments
- Isolate applications from each other

Benefits:
- "Works on my machine" problem solved
- Faster deployment
- Efficient resource usage
- Easy scaling
```

### 2. What's the difference between Image and Container?

**Expected Answer:**
```
IMAGE:
- Blueprint/template
- Read-only
- Stored on disk
- Reusable

CONTAINER:
- Running instance
- Can be modified
- Uses RAM
- Temporary (can be deleted)

Analogy:
Image = Recipe
Container = Cooked meal
```

### 3. What is Docker Hub?

**Expected Answer:**
```
Docker Hub is:
- Central repository for Docker images
- Like GitHub for images
- Contains official images (Python, Node, MySQL)
- Contains community images
- Used with "docker pull" and "docker push"
- Free public repositories
- Paid private repositories
```

### 4. How do you run a Docker container?

**Expected Answer:**
```bash
$ docker run [OPTIONS] IMAGE [COMMAND]

Examples:
$ docker run python:3.9          # Basic
$ docker run -d nginx            # Background
$ docker run -p 8080:80 nginx    # Port mapping
$ docker run -v /host:/app image # Volume
$ docker run -e KEY=value image  # Environment
$ docker run -it ubuntu bash     # Interactive
```

### 5. What's the difference between docker stop and docker kill?

**Expected Answer:**
```
docker stop:
- Graceful shutdown
- Sends SIGTERM signal
- Container gets time to cleanup
- Takes a few seconds
- Recommended for production

docker kill:
- Immediate termination
- Sends SIGKILL signal
- No cleanup time
- Instantaneous
- Use only when necessary
```

### 6. What is a Dockerfile?

**Expected Answer:**
```
Dockerfile is:
- Text file with instructions
- Used to create Docker images
- Contains steps to build image
- FROM, RUN, COPY, EXPOSE, etc.
- Like setup script

Example:
FROM python:3.9
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
CMD ["python", "app.py"]
```

### 7. How do you build a Docker image?

**Expected Answer:**
```bash
$ docker build -t image-name:tag .

Breakdown:
-t          Tag name
image-name  Name of image
tag         Version (latest if not specified)
.           Current directory (location of Dockerfile)

Examples:
$ docker build -t myapp:1.0 .
$ docker build -f Dockerfile.prod -t app:1.0 .
$ docker build --no-cache -t app:1.0 .
```

### 8. What are Docker volumes?

**Expected Answer:**
```
Volumes are:
- Persistent data storage
- Survive container deletion
- Can be shared between containers
- Two types:
  1. Named volumes (Docker-managed)
  2. Bind mounts (Host folder)

Use cases:
- Database data persistence
- Application logs
- Configuration files
- Shared data between containers

Example:
$ docker run -v mydata:/data image
```

### 9. What is port mapping in Docker?

**Expected Answer:**
```
Port mapping connects:
- Host port (your computer)
- Container port (inside container)

Syntax: -p host-port:container-port

Example:
$ docker run -p 8080:80 nginx
Means: Port 8080 on your PC → Port 80 in container

Use:
- Access container services from outside
- Run multiple apps on different ports
- Hide internal port numbers
```

### 10. How do you view container logs?

**Expected Answer:**
```bash
# View all logs
$ docker logs container-id

# Follow logs (like tail -f)
$ docker logs -f container-id

# Last 50 lines
$ docker logs --tail 50 container-id

# With timestamps
$ docker logs -t container-id

# Since specific time
$ docker logs --since 2024-01-15T10:00:00 container-id
```

---

## Intermediate Level

### 11. What is Docker Compose and when do you use it?

**Expected Answer:**
```
Docker Compose is:
- Tool for multi-container applications
- Define services in docker-compose.yml
- Start all containers with one command

Use cases:
- Development environment
- Testing environment
- Multi-service applications (web + database + cache)

Advantages:
- Single command: docker-compose up
- Easy service orchestration
- Automatic networking
- Environment variables management
- Volume management

Command:
$ docker-compose up -d
$ docker-compose ps
$ docker-compose down
```

### 12. Explain Docker networking

**Expected Answer:**
```
Docker networking types:

1) Bridge (default)
   - Containers on same bridge can communicate
   - Isolated from host network
   - Good for development

2) Host
   - Container uses host network
   - No isolation
   - Best performance
   - Limited isolation

3) Overlay
   - Multi-host communication
   - For Docker Swarm/Kubernetes
   - Cross-machine networking

4) Macvlan
   - Assign MAC addresses to containers
   - Like separate physical machines
   - Advanced use cases

5) None
   - No network
   - Container isolated

Default behavior:
- Containers can communicate on same network
- Use service name as hostname
- Example: http://database:3306
```

### 13. What are Docker layers and why are they important?

**Expected Answer:**
```
Layers:
- Each Dockerfile instruction creates a layer
- Layers are stacked to create image
- Each layer is read-only

Benefits:
1) Efficiency
   - Share layers between images
   - Save disk space

2) Caching
   - Docker caches layers
   - Rebuild faster
   - Only changed layers rebuild

3) Reusability
   - Same base layer in multiple images
   - Reduce redundancy

Example:
Layer 1: Base OS (Ubuntu)
Layer 2: Python installed
Layer 3: Dependencies (pip install)
Layer 4: Your code
Layer 5: Startup commands

Stacked together = Complete image
```

### 14. What are environment variables in Docker and how do you use them?

**Expected Answer:**
```
Environment variables:
- Configuration values
- Passed to container
- Can be changed without rebuilding

Methods to pass:

1) -e flag
$ docker run -e KEY=value image

2) Multiple variables
$ docker run -e KEY1=value1 -e KEY2=value2 image

3) From file
$ docker run --env-file .env image

4) In docker-compose.yml
services:
  app:
    environment:
      - KEY=value
      - DEBUG=true

Use cases:
- Database credentials
- API keys
- Configuration
- Logging levels
```

### 15. How do you debug a Docker container?

**Expected Answer:**
```
Debugging steps:

1) Check if running
$ docker ps | grep container-name

2) View logs
$ docker logs container-id
$ docker logs -f container-id

3) Get shell access
$ docker exec -it container-id bash

4) Inspect container
$ docker inspect container-id

5) Check resources
$ docker stats container-id

6) View processes
$ docker top container-id

7) Check network
$ docker port container-id

8) Check configuration
$ docker inspect -f '{{json .Config}}' container-id
```

### 16. What are best practices for Dockerfiles?

**Expected Answer:**
```
Best practices:

1) Use minimal base images
   FROM alpine:latest  (5MB)
   NOT: FROM ubuntu:latest (77MB)

2) Combine RUN commands
   BAD:
   RUN apt-get update
   RUN apt-get install -y python

   GOOD:
   RUN apt-get update && apt-get install -y python

3) Use .dockerignore
   Exclude unnecessary files

4) Minimize layers
   Each layer = larger image

5) Use specific versions
   FROM python:3.9
   NOT: FROM python:latest

6) Run as non-root user
   USER appuser

7) Use ENTRYPOINT + CMD
   ENTRYPOINT ["python"]
   CMD ["app.py"]

8) Keep images small
   Use multi-stage builds
   Delete package manager cache
```

### 17. What is multi-stage build and why use it?

**Expected Answer:**
```
Multi-stage build:
- Use multiple FROM statements
- Build in one stage, use in another
- Reduce final image size

Example:
# Stage 1: Build
FROM node:14 as builder
WORKDIR /app
COPY . .
RUN npm install
RUN npm run build

# Stage 2: Runtime
FROM node:14-alpine
WORKDIR /app
COPY --from=builder /app/dist ./
CMD ["node", "index.js"]

Benefits:
- Smaller image (only runtime needed)
- Faster deployment
- No build tools in production
- Improved security
```

### 18. How do you persist data in Docker?

**Expected Answer:**
```
Methods:

1) Volumes
   $ docker run -v mydata:/data image
   - Docker-managed
   - Persistent
   - Shareable

2) Bind mounts
   $ docker run -v /host/path:/container/path image
   - Mount host directory
   - Two-way sync
   - Development

3) tmpfs
   $ docker run --tmpfs /temp image
   - In-memory storage
   - Fast but temporary
   - Lost on stop

Best practices:
- Use volumes for databases
- Use bind mounts for development
- Use tmpfs for temporary data
- Never store data in container layer
```

### 19. What is Docker networking between containers?

**Expected Answer:**
```
Container-to-container communication:

Same network (automatic):
- Containers on same network can communicate
- Use service name as hostname
- No port exposure needed (internal)

Example:
web container can reach database as:
http://database:3306

In docker-compose:
- All services on same network by default
- Services use service name to communicate
- No explicit networking needed

Commands:
$ docker network create mynet
$ docker run --network mynet web-image
$ docker run --network mynet db-image

Isolation:
- Different networks = no communication
- Same network = direct communication
- Can create multiple networks
```

### 20. How do you scale containers?

**Expected Answer:**
```
Scaling methods:

1) Docker Compose (simple)
$ docker-compose up --scale web=3
- Runs 3 instances of web service

2) Docker Swarm (native)
$ docker service create --replicas 3 image

3) Kubernetes (production)
replicas: 3
- Industry standard
- Auto-scaling
- Load balancing

5) Manual scaling (not recommended)
- Run multiple containers manually
- Manage load balancing yourself

Load balancing:
- Need external load balancer
- Or use reverse proxy (nginx)
- Or use orchestration tool
```

---

## Advanced Level

### 21. What is Docker security best practices?

**Expected Answer:**
```
Security practices:

1) Image security
   - Use trusted images
   - Scan images for vulnerabilities
   - Use specific versions (not latest)
   - Minimal base images
   - Run as non-root user

2) Container security
   - Run with read-only filesystem
   - Drop unnecessary capabilities
   - Use security options
   - Network isolation

3) Registry security
   - Use private registry
   - Authenticate access
   - Sign images
   - Use TLS/HTTPS

4) Runtime security
   - Monitor containers
   - Limit resources
   - Regular updates
   - Audit logs

Example secure Dockerfile:
FROM alpine:3.14
RUN addgroup -S appgroup
RUN adduser -S appuser -G appgroup
COPY --chown=appuser:appgroup . /app
USER appuser
CMD ["./app"]
```

### 22. How do you monitor Docker containers?

**Expected Answer:**
```
Monitoring tools:

1) Built-in
$ docker stats
$ docker ps
$ docker logs

2) Metrics collection
- Prometheus (metrics)
- cAdvisor (container metrics)
- Telegraf

3) Visualization
- Grafana (dashboards)
- Datadog
- New Relic

4) Logging
- ELK stack (Elasticsearch, Logstash, Kibana)
- Splunk
- CloudWatch (AWS)

What to monitor:
- CPU usage
- Memory usage
- Disk I/O
- Network I/O
- Container logs
- Container restarts
- Health checks

Health checks in Docker:
HEALTHCHECK --interval=30s --timeout=5s \
  CMD curl http://localhost:8080
```

### 23. What is Docker registry and how do you set it up?

**Expected Answer:**
```
Registry:
- Storage for images
- Centralized image repository

Types:

1) Docker Hub (public)
- Free public images
- Commercial private images

2) Private Registry (on-premise)
$ docker run -d -p 5000:5000 registry:2
- Self-hosted
- Complete control
- For security/compliance

3) Cloud Registries
- AWS ECR (Elastic Container Registry)
- Google Container Registry
- Azure Container Registry

Push to private registry:
$ docker tag myimage:1.0 localhost:5000/myimage:1.0
$ docker push localhost:5000/myimage:1.0

Pull from private registry:
$ docker pull localhost:5000/myimage:1.0
```

### 24. How do you implement CI/CD with Docker?

**Expected Answer:**
```
CI/CD pipeline with Docker:

1) Build stage
   - Pull code
   - Build Docker image
   - Tag with version

2) Test stage
   - Run container
   - Run tests inside
   - Test image

3) Push stage
   - Push to registry
   - Tag as latest

4) Deploy stage
   - Pull from registry
   - Start container
   - Verify health

Tools:
- Jenkins
- GitLab CI
- GitHub Actions
- Travis CI

Example GitHub Actions:
name: CI/CD
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Build image
        run: docker build -t app:latest .
      - name: Run tests
        run: docker run app:latest npm test
      - name: Push image
        run: docker push myrepo/app:latest
```

### 25. What are common Docker production issues?

**Expected Answer:**
```
Common issues:

1) Container crashes
   - Check logs: docker logs
   - Check exit code: docker ps -a
   - Inspect: docker inspect
   - Solution: Fix application, rebuild

2) Disk space
   - Remove unused images/containers
   - Prune: docker system prune
   - Check: du -sh /var/lib/docker
   - Solution: Cleanup, use storage driver

3) Performance
   - Monitor: docker stats
   - Resource limits too low
   - Image too large
   - Solution: Optimize, add resources

4) Networking
   - Can't reach services
   - Wrong network
   - Port conflicts
   - Solution: Check network, inspect

5) Security issues
   - Vulnerabilities
   - Running as root
   - Exposed secrets
   - Solution: Scan images, use secrets

6) Memory leaks
   - Container memory grows
   - Application issue
   - Monitor over time
   - Solution: Fix application, restart
```

---

## Company-Specific Questions

### Google/AWS/Microsoft Questions

```
1. How would you deploy this on Kubernetes?
2. Explain container orchestration
3. How do you handle multi-region deployment?
4. What about disaster recovery?
5. How do you manage secrets?
6. Container registry strategy?
7. Cost optimization with containers?
8. Auto-scaling strategies?
```

### DevOps-Specific

```
1. How do you manage environment configs?
2. CI/CD pipeline design
3. Monitoring and logging strategy
4. Backup and recovery
5. Security scanning
6. Infrastructure as Code (Terraform, Ansible)
7. Container networking design
```

---

## Tips for Interviews

### General Tips 💡

```
1. Understand the concepts
   - Not just commands
   - Why we use Docker
   - When to use Docker

2. Provide examples
   - Real-world scenarios
   - Concrete use cases
   - Show experience

3. Be honest
   - Don't pretend to know
   - Say "I don't know but..."
   - Show willingness to learn

4. Ask questions
   - Clarify ambiguous questions
   - Show interest
   - Understand requirements

5. Think out loud
   - Explain your thought process
   - Show problem-solving
   - Be interactive

6. Practice
   - Run through questions
   - Have real examples
   - Use Docker daily

7. Current knowledge
   - Docker newest features
   - Industry trends
   - What you actually use
```

### Technical Interview Preparation 🔧

```
Setup local environment:
1. Install Docker
2. Practice building images
3. Run containers
4. Use Docker Compose
5. Try networking
6. Work with volumes
7. Practice debugging

Have these ready:
- Your own Docker images
- docker-compose.yml examples
- Real project examples
- Common errors & solutions
```

### Common Follow-up Questions

```
1. Explain your last Docker project
2. What challenges did you face?
3. How did you optimize image size?
4. Production deployment experience?
5. Debugging complex issues
6. Performance optimization
7. Security implementations
8. Team collaboration with Docker
```

---

## Answer Structure

### Good Answer Format

```
1. Answer directly
   "Yes, I would use volumes for..."

2. Explain why
   "Because volumes persist data..."

3. Give example
   "For example, docker run -v mydata:/data..."

4. Mention alternatives
   "Other options include bind mounts..."

5. Ask clarifying questions
   "Would this be for development or production?"
```

---

**Good luck with your interview!** 🎤✨

Last updated: January 2025  
Difficulty: All Levels  
Questions: 50+
