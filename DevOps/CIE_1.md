# Docker & DevOps - Comprehensive Study Notes

## Table of Contents

1. [The DevOps Culture](https://claude.ai/chat/7dada141-9b44-4e56-b6d0-6850b3ee6cea#1-the-devops-culture)
2. [Docker Fundamentals](https://claude.ai/chat/7dada141-9b44-4e56-b6d0-6850b3ee6cea#2-docker-fundamentals)
3. [Building Docker Applications](https://claude.ai/chat/7dada141-9b44-4e56-b6d0-6850b3ee6cea#3-building-docker-applications)
4. [Docker Architecture Deep Dive](https://claude.ai/chat/7dada141-9b44-4e56-b6d0-6850b3ee6cea#4-docker-architecture-deep-dive)
5. [10 Essential Docker Techniques](https://claude.ai/chat/7dada141-9b44-4e56-b6d0-6850b3ee6cea#5-ten-essential-docker-techniques)

---

## 1. The DevOps Culture

### 1.1 What is DevOps?

**DevOps** is a culture, movement, and practice that emphasizes collaboration between software developers (Dev) and IT operations (Ops) teams. It aims to shorten the systems development lifecycle and provide continuous delivery with high software quality.

### 1.2 The DevOps Lifecycle

```mermaid
graph LR
    A[Plan] --> B[Code]
    B --> C[Build]
    C --> D[Test]
    D --> E[Release]
    E --> F[Deploy]
    F --> G[Operate]
    G --> H[Monitor]
    H --> A
    
    style A fill:#e1f5ff
    style E fill:#ffe1e1
    style G fill:#e1ffe1
```

#### Lifecycle Stages Explained:

|Stage|Description|Key Activities|Tools/Technologies|
|---|---|---|---|
|**Plan**|Project planning and requirement gathering|Sprint planning, backlog management|Jira, Trello, Azure Boards|
|**Code**|Source code development|Version control, code review|Git, GitHub, GitLab, Bitbucket|
|**Build**|Automated compilation and packaging|CI pipelines, dependency management|Maven, Gradle, npm|
|**Test**|Quality assurance and testing|Unit tests, integration tests|JUnit, Selenium, Jest|
|**Release**|CI/CD implementation|Automated releases, artifact management|Jenkins, CircleCI, Travis CI|
|**Deploy**|Infrastructure provisioning|IaC, configuration management|Terraform, Ansible, Chef, Puppet|
|**Operate**|Runtime management|Containerization, orchestration|Docker, Kubernetes|
|**Monitor**|Performance tracking|Logging, metrics, alerts|Prometheus, Grafana, ELK Stack|

### 1.3 DevOps vs Traditional Approach

```mermaid
flowchart TB
    subgraph Traditional["Traditional Approach"]
        direction TB
        Dev1[Development Team] -->|Handoff| Ops1[Operations Team]
        Ops1 -->|Issues| Dev1
        style Dev1 fill:#ffcccc
        style Ops1 fill:#ccccff
    end
    
    subgraph DevOps["DevOps Approach"]
        direction TB
        Team[Unified DevOps Team]
        Team -->|Collaborate| Auto[Automation]
        Auto -->|Feedback| Team
        style Team fill:#ccffcc
        style Auto fill:#ffffcc
    end
```

### 1.4 Key DevOps Principles

1. **Automation** - Automate repetitive tasks
2. **Continuous Integration/Continuous Delivery (CI/CD)** - Frequent code integration and deployment
3. **Collaboration** - Break down silos between teams
4. **Monitoring & Feedback** - Continuous performance tracking
5. **Infrastructure as Code (IaC)** - Manage infrastructure through code

---

## 2. Docker Fundamentals

### 2.1 What is Docker?

**Docker** is an open-source platform that automates the deployment, scaling, and management of applications using containerization technology. It packages applications and their dependencies into standardized units called **containers**.

### 2.2 Life Before Docker vs Life With Docker

#### Before Docker:

```mermaid
graph TD
    subgraph "Physical/Virtual Machines"
        A[Application 1] --> B[OS 1]
        C[Application 2] --> D[OS 2]
        E[Application 3] --> F[OS 3]
        B --> G[Hardware]
        D --> G
        F --> G
    end
    
    style B fill:#ff9999
    style D fill:#ff9999
    style F fill:#ff9999
```

**Problems:**

- Heavy resource consumption (each VM needs full OS)
- Slow startup times (minutes)
- Environment inconsistencies ("works on my machine")
- Complex dependency management

#### With Docker:

```mermaid
graph TD
    subgraph "Docker Containers"
        A[Container 1] --> E[Docker Engine]
        B[Container 2] --> E
        C[Container 3] --> E
        D[Container N] --> E
        E --> F[Host OS]
        F --> G[Hardware]
    end
    
    style E fill:#99ff99
    style F fill:#9999ff
```

**Benefits:**

- Lightweight (containers share host OS kernel)
- Fast startup (seconds)
- Consistent environments
- Efficient resource utilization
- Easy scaling and orchestration

### 2.3 What is Docker Good For?

#### 1. **Microservices Architecture**

Breaking monolithic applications into smaller, independent services.

```mermaid
graph LR
    A[API Gateway] --> B[User Service]
    A --> C[Payment Service]
    A --> D[Order Service]
    B --> E[(Database)]
    C --> F[(Database)]
    D --> G[(Database)]
    
    style B fill:#e1f5ff
    style C fill:#ffe1f5
    style D fill:#f5ffe1
```

#### 2. **Replacing Virtual Machines**

- Lighter weight than VMs
- Faster provisioning
- Better resource utilization

#### 3. **Enabling Continuous Deployment (CD)**

- Consistent deployment pipeline
- Easy rollback mechanisms
- Blue-green deployments

#### 4. **Modeling Networks**

- Create isolated network environments
- Test distributed systems locally

#### 5. **Packaging Software**

- "Build once, run anywhere"
- Standardized distribution format
- Version control for infrastructure

### 2.4 Key Docker Concepts

#### **Container**

A runnable instance of an image. It's an isolated process with its own filesystem, networking, and process tree.

```bash
# Example: Running a container
docker run -it ubuntu:latest /bin/bash
```


#### **Image**

A read-only template containing instructions for creating a container. Images are built from Dockerfiles.

```bash
# Example: Listing images
docker images

# Output:
# REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
# ubuntu        latest    fb52e22af1b0   2 weeks ago    72.8MB
```

#### **Layers**

Docker images are built in layers. Each instruction in a Dockerfile creates a new layer, enabling efficient caching and reuse.

```mermaid
graph TB
    A[Base Image Layer - ubuntu:latest] --> B[Layer 2 - RUN apt-get update]
    B --> C[Layer 3 - RUN apt-get install nginx]
    C --> D[Layer 4 - COPY index.html]
    D --> E[Layer 5 - CMD nginx]
    
    style A fill:#e1f5ff
    style E fill:#ffe1e1
```

### 2.5 Essential Docker Commands

```bash
# BUILD - Create an image from Dockerfile
docker build -t myapp:v1 .
# -t: Tag the image with name and version
# .: Build context (current directory)

# RUN - Create and start a container
docker run -d -p 8080:80 --name mycontainer nginx
# -d: Detached mode (run in background)
# -p: Port mapping (host:container)
# --name: Assign a name to container

# COMMIT - Create a new image from container's changes
docker commit mycontainer myapp:v2
# Captures current state of container as new image

# TAG - Create a new tag for an image
docker tag myapp:v1 myapp:latest
# Useful for versioning and registry pushing

# PS - List running containers
docker ps
# Add -a flag to see all containers (including stopped)

# IMAGES - List all images
docker images

# STOP - Stop a running container
docker stop mycontainer

# START - Start a stopped container
docker start mycontainer

# RM - Remove a container
docker rm mycontainer

# RMI - Remove an image
docker rmi myapp:v1
```

---

## 3. Building Docker Applications

### 3.1 Ways to Create a New Docker Image

|Method|Description|Use Case|Complexity|
|---|---|---|---|
|**Docker Commands (Manual)**|Run container, make changes, commit|Quick prototyping, testing|Low|
|**Dockerfile**|Scripted build from base image|Production, reproducible builds|Medium|
|**Dockerfile + CM Tool**|Use config management (Ansible/Puppet)|Complex infrastructure|High|
|**Scratch + TAR Import**|Build from empty image|Minimal images, security|High|

### 3.2 Writing a Dockerfile

A **Dockerfile** is a text document containing commands to assemble an image. Each instruction creates a layer in the final image.

#### Dockerfile Syntax and Best Practices:

```dockerfile
# 1. BASE IMAGE - Always start with a base image
FROM node:14-alpine
# Use specific tags, avoid 'latest' for production
# Alpine variants are smaller (~5MB vs ~900MB)

# 2. METADATA - Document the image
LABEL maintainer="yourname@example.com"
LABEL version="1.0"
LABEL description="Todo Application"

# 3. WORKING DIRECTORY - Set working directory for subsequent commands
WORKDIR /app
# Creates directory if it doesn't exist

# 4. COPY FILES - Copy files from build context to image
COPY package*.json ./
# Copy dependency files first (better layer caching)

# 5. RUN COMMANDS - Execute commands during build
RUN npm install --production
# Combines commands with && to reduce layers:
# RUN apt-get update && apt-get install -y curl && apt-get clean

# 6. COPY APPLICATION - Copy source code
COPY . .
# Copies everything from build context

# 7. EXPOSE PORTS - Document which ports the container listens on
EXPOSE 8000
# This is documentation only, actual publishing done at runtime

# 8. USER - Set user for security (avoid running as root)
USER node

# 9. VOLUME - Create mount points
VOLUME ["/app/data"]
# For persistent data

# 10. CMD - Default command when container starts
CMD ["npm", "start"]
# Only one CMD per Dockerfile
# Can be overridden at runtime

# Alternative: ENTRYPOINT for commands that shouldn't be overridden
# ENTRYPOINT ["npm"]
# CMD ["start"]
```

#### Complete Example: Todo Application

```dockerfile
# Todo Application Dockerfile

# Use official Node.js alpine image (lightweight)
FROM node:14-alpine

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm install --production && \
    npm cache clean --force

# Copy application source
COPY . .

# Create non-root user for security
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001 && \
    chown -R nodejs:nodejs /app

# Switch to non-root user
USER nodejs

# Expose port
EXPOSE 8000

# Health check
HEALTHCHECK --interval=30s --timeout=3s \
  CMD node healthcheck.js || exit 1

# Start application
CMD ["npm", "start"]
```

### 3.3 Building a Docker Image

```bash
# Basic build command
docker build -t todoapp:v1 .

# Build with custom Dockerfile name
docker build -f Dockerfile.prod -t todoapp:prod .

# Build with build arguments
docker build --build-arg NODE_ENV=production -t todoapp:v1 .

# Build without cache (fresh build)
docker build --no-cache -t todoapp:v1 .

# Build with multiple tags
docker build -t todoapp:v1 -t todoapp:latest .
```

**Build Process Flow:**

```mermaid
sequenceDiagram
    participant User
    participant Docker Client
    participant Docker Daemon
    participant Registry
    
    User->>Docker Client: docker build -t myapp .
    Docker Client->>Docker Daemon: Send build context
    Docker Daemon->>Registry: Pull base image (if needed)
    Registry-->>Docker Daemon: Return base image
    Docker Daemon->>Docker Daemon: Execute Dockerfile instructions
    Docker Daemon->>Docker Daemon: Create image layers
    Docker Daemon-->>Docker Client: Build complete
    Docker Client-->>User: Success message with image ID
```

### 3.4 Running a Docker Container

```bash
# Basic run command
docker run todoapp:v1

# Interactive mode with terminal
docker run -it todoapp:v1 /bin/bash
# -i: Keep STDIN open
# -t: Allocate pseudo-TTY

# Detached mode with port mapping
docker run -d -p 8000:8000 --name todo todoapp:v1
# -d: Run in background
# -p: Map host port 8000 to container port 8000
# --name: Give container a friendly name

# With environment variables
docker run -d -p 8000:8000 \
  -e NODE_ENV=production \
  -e DB_HOST=mysql \
  --name todo todoapp:v1

# With volume mounting
docker run -d -p 8000:8000 \
  -v $(pwd)/data:/app/data \
  --name todo todoapp:v1
# Mounts host directory to container

# With resource limits
docker run -d -p 8000:8000 \
  --memory="512m" \
  --cpus="1.5" \
  --name todo todoapp:v1

# With restart policy
docker run -d -p 8000:8000 \
  --restart=unless-stopped \
  --name todo todoapp:v1
# Restart policies: no, always, on-failure, unless-stopped
```

**Container Lifecycle Management:**

```bash
# View running containers
docker ps

# View all containers (including stopped)
docker ps -a

# View container logs
docker logs todo
docker logs -f todo  # Follow logs (real-time)

# View container stats
docker stats todo

# Execute command in running container
docker exec -it todo /bin/bash

# Stop container gracefully
docker stop todo
# Sends SIGTERM, waits 10s, then SIGKILL

# Force stop container
docker kill todo

# Restart container
docker restart todo

# Pause/unpause container
docker pause todo
docker unpause todo

# Remove container
docker rm todo
docker rm -f todo  # Force remove running container

# View container details
docker inspect todo

# View container processes
docker top todo

# View container file changes
docker diff todo
```

### 3.5 Docker Layering in Detail

Docker uses a **Union File System** (UnionFS) to build images in layers. Each instruction in a Dockerfile creates a new layer.

```mermaid
graph TB
    subgraph "Docker Image Layers"
        L1[Layer 1: FROM ubuntu:20.04<br/>Base Image - 72MB]
        L2[Layer 2: RUN apt-get update<br/>Package Lists - 25MB]
        L3[Layer 3: RUN apt-get install nginx<br/>Nginx Installation - 15MB]
        L4[Layer 4: COPY index.html<br/>Static File - 2KB]
        L5[Layer 5: CMD nginx<br/>Metadata - 0 bytes]
        
        L1 --> L2
        L2 --> L3
        L3 --> L4
        L4 --> L5
    end
    
    L5 --> C[Container Layer<br/>Writable Layer]
    
    style L1 fill:#e1f5ff
    style L2 fill:#e8e1ff
    style L3 fill:#ffe1f5
    style L4 fill:#f5ffe1
    style L5 fill:#ffe1e1
    style C fill:#ffeecc
```

#### Key Concepts:

1. **Read-Only Layers**: Image layers are immutable
2. **Writable Container Layer**: Each container gets a thin writable layer on top
3. **Copy-on-Write (CoW)**: Files modified in container are copied to writable layer
4. **Layer Caching**: Docker caches layers to speed up builds

#### Optimizing Dockerfile for Better Layering:

```dockerfile
# ❌ BAD - Creates multiple layers, poor caching
FROM ubuntu:20.04
RUN apt-get update
RUN apt-get install -y nginx
RUN apt-get install -y curl
COPY . /app

# ✅ GOOD - Fewer layers, better caching
FROM ubuntu:20.04
# Combine related commands
RUN apt-get update && \
    apt-get install -y nginx curl && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Copy dependencies first (changes less frequently)
COPY package*.json /app/
RUN cd /app && npm install

# Copy source code last (changes frequently)
COPY . /app
```

#### Viewing Image Layers:

```bash
# View image history (layers)
docker history todoapp:v1

# Output:
# IMAGE          CREATED          CREATED BY                                      SIZE
# a1b2c3d4e5f6   2 minutes ago    CMD ["npm" "start"]                             0B
# b2c3d4e5f6g7   2 minutes ago    COPY . /app                                     1.2MB
# c3d4e5f6g7h8   3 minutes ago    RUN npm install                                 45MB
# d4e5f6g7h8i9   3 minutes ago    COPY package*.json /app/                        12KB
# e5f6g7h8i9j0   10 days ago      WORKDIR /app                                    0B
# node:14-alpine 2 weeks ago      <base image>                                    116MB

# Analyze image size and layers
docker inspect todoapp:v1

# View layer details with dive tool (if installed)
dive todoapp:v1
```

---

## 4. Docker Architecture Deep Dive

### 4.1 Docker's Architecture Overview

Docker uses a **client-server architecture**. The Docker client communicates with the Docker daemon via REST API over HTTP.

```mermaid
graph TB
    subgraph "Your Host Machine"
        Client[Docker Client<br/>docker CLI]
        Daemon[Docker Daemon<br/>dockerd]
        Client -->|HTTP/REST API| Daemon
    end
    
    subgraph "Network"
        Registry1[Docker Hub<br/>Public Registry]
        Registry2[Private Registry<br/>Your Organization]
        Registry3[Other Registries<br/>Quay.io, ECR, etc.]
    end
    
    Daemon -->|Pull/Push Images| Registry1
    Daemon -->|Pull/Push Images| Registry2
    Daemon -->|Pull/Push Images| Registry3
    
    Daemon --> Containers[Containers]
    Daemon --> Images[Images]
    Daemon --> Networks[Networks]
    Daemon --> Volumes[Volumes]
    
    style Client fill:#e1f5ff
    style Daemon fill:#ffe1e1
    style Registry1 fill:#e1ffe1
```

### 4.2 The Docker Daemon (dockerd)

The **Docker daemon** (`dockerd`) is the persistent background process that manages Docker containers, images, networks, and volumes.

#### Key Responsibilities:

1. **Container Management** - Create, start, stop, remove containers
2. **Image Management** - Build, pull, push, remove images
3. **Network Management** - Create and manage container networks
4. **Volume Management** - Manage persistent data
5. **API Server** - Listen for Docker API requests

#### Docker Daemon Configuration:

```bash
# View daemon configuration
docker info

# Daemon runs as a service (systemd on Linux)
sudo systemctl status docker

# Start/stop daemon
sudo systemctl start docker
sudo systemctl stop docker

# Enable daemon on boot
sudo systemctl enable docker
```

#### Daemon Communication:

```mermaid
sequenceDiagram
    participant Client as Docker Client
    participant Socket as Unix Socket<br/>/var/run/docker.sock
    participant Daemon as Docker Daemon
    
    Client->>Socket: docker ps
    Socket->>Daemon: HTTP GET /containers/json
    Daemon->>Daemon: List containers
    Daemon-->>Socket: JSON response
    Socket-->>Client: Display containers
```

### 4.3 The Docker Client

The **Docker client** (`docker`) is the primary interface for users to interact with Docker. When you run commands like `docker run`, the client sends these commands to `dockerd`.

#### Client-Daemon Communication:

```bash
# Default: Unix socket
docker ps
# Uses /var/run/docker.sock

# TCP connection (remote Docker host)
docker -H tcp://192.168.1.100:2375 ps

# Multiple hosts
docker -H tcp://host1:2375 ps
docker -H tcp://host2:2375 ps
```

### 4.4 Docker Registries

A **Docker registry** stores Docker images. Docker Hub is the default public registry, but you can run private registries.

```mermaid
graph LR
    A[Developer] -->|docker push| B[Registry]
    B -->|docker pull| C[Production Server]
    B -->|docker pull| D[Test Server]
    
    style B fill:#ffe1e1
```

#### Registry Types:

1. **Public Registries**
    
    - Docker Hub (hub.docker.com)
    - Quay.io
    - GitHub Container Registry
2. **Private Registries**
    
    - Self-hosted Docker Registry
    - Harbor
    - Amazon ECR
    - Google GCR
    - Azure ACR

#### Working with Registries:

```bash
# Login to registry
docker login
# For private registries
docker login myregistry.com

# Tag image for registry
docker tag myapp:v1 username/myapp:v1
docker tag myapp:v1 myregistry.com/myapp:v1

# Push image to registry
docker push username/myapp:v1

# Pull image from registry
docker pull username/myapp:v1

# Search Docker Hub
docker search nginx
```

### 4.5 The Docker Hub

**Docker Hub** is Docker's official cloud-based registry service.

**Features:**

- Public and private repositories
- Automated builds
- Webhooks
- Official images (verified by Docker)
- Team collaboration

```bash
# Pull official image
docker pull nginx:latest

# Pull specific version
docker pull nginx:1.21-alpine

# Pull from user repository
docker pull username/myapp:v1
```

---

## 5. Ten Essential Docker Techniques

### Technique 1: Exposing Docker Daemon Over TCP

**Problem:** You want to open your Docker server for remote access.

**Solution:** Configure the Docker daemon to listen on a TCP port.

```mermaid
graph LR
    A[Local Machine] -->|TCP 2375| B[Docker Daemon]
    C[Jenkins Server] -->|TCP 2375| B
    D[Another Machine] -->|TCP 2375| B
    
    style B fill:#ffe1e1
```

#### Implementation Steps:

```bash
# Step 1: Check Docker status
sudo systemctl status docker

# Step 2: Edit systemd service file
sudo nano /lib/systemd/system/docker.service

# Step 3: Comment out existing ExecStart line
# ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

# Step 4: Add new ExecStart with TCP binding
# ExecStart=/usr/bin/dockerd -H=fd:// -H=tcp://0.0.0.0:2375

# Step 5: Reload and restart Docker
sudo systemctl daemon-reload
sudo systemctl restart docker

# Step 6: Check IP address
ifconfig

# Step 7: Test via web browser
# http://YOUR_IP:2375/images/json

# Step 8: Access from remote machine
curl http://YOUR_IP:2375/images/json
```

**Configuration Explanation:**

```bash
# -H=fd:// : Listen on file descriptor (systemd)
# -H=tcp://0.0.0.0:2375 : Listen on all interfaces, port 2375
# 0.0.0.0 means accept connections from any IP
# 2375 is the default unencrypted Docker port
# 2376 is for encrypted TLS connections
```

⚠️ **Security Warning:**

```
Opening Docker daemon over TCP without TLS is INSECURE!
Anyone who can connect has full control over your Docker host.

For production, use TLS encryption:
ExecStart=/usr/bin/dockerd \
  --tlsverify \
  --tlscacert=/etc/docker/ca.pem \
  --tlscert=/etc/docker/server-cert.pem \
  --tlskey=/etc/docker/server-key.pem \
  -H=tcp://0.0.0.0:2376
```

**Use Cases:**

- Remote Docker management
- CI/CD integration (Jenkins, GitLab CI)
- Docker cluster management
- Development/testing environments

---

### Technique 2: Running Containers as Daemons

**Problem:** You want to run containers as background services.

**Solution:** Use the `-d` (detached) flag with `docker run`.

#### Implementation:

```bash
# Step 1: Create project directory
mkdir secondfile && cd secondfile

# Step 2: Create Dockerfile
nano Dockerfile
```

```dockerfile
# Dockerfile content
FROM alpine

# Install Redis
RUN apk add --update redis

# Start Redis server
CMD ["redis-server"]
```

```bash
# Step 3: Build the image
docker build -t secondfile .

# Step 4: Check images
docker images

# Step 5: Run in foreground (blocks terminal)
docker run secondfile
# Press Ctrl+C to stop

# Step 6: Run in detached mode (background)
docker run -d secondfile
# Returns container ID immediately

# Step 7: Check running containers
docker ps

# Step 8: View container logs
docker logs <container_id>

# Step 9: Follow logs in real-time
docker logs -f <container_id>

# Step 10: Stop daemon container
docker stop <container_id>
```

#### Daemon vs Foreground Comparison:

|Feature|Foreground (-it)|Daemon (-d)|
|---|---|---|
|Terminal|Blocked|Free|
|Logs|Direct output|Use `docker logs`|
|Stop|Ctrl+C|`docker stop`|
|Use Case|Debugging|Production services|

#### Advanced Daemon Options:

```bash
# Run with restart policy
docker run -d --restart=always redis

# Restart policies:
# no         - Don't restart (default)
# always     - Always restart
# on-failure - Restart on non-zero exit
# unless-stopped - Restart unless manually stopped

# Run with health checks
docker run -d \
  --health-cmd="redis-cli ping" \
  --health-interval=30s \
  --health-timeout=3s \
  --health-retries=3 \
  redis

# Run with logging options
docker run -d \
  --log-driver=json-file \
  --log-opt max-size=10m \
  --log-opt max-file=3 \
  redis
```

**Real-World Applications:**

- Web servers (nginx, Apache)
- Databases (MySQL, PostgreSQL, MongoDB)
- Message queues (RabbitMQ, Kafka)
- Caching services (Redis, Memcached)
- Microservices

---

### Technique 3: Moving Docker to a Different Partition

**Problem:** Docker's default data directory (`/var/lib/docker`) is running out of space.

**Solution:** Change Docker's data root directory to a partition with more space.

```mermaid
graph LR
    A[Old Location<br/>/var/lib/docker] -.X.-> B[Limited Space]
    C[New Location<br/>/home/temp_docker] --> D[More Space]
    
    style A fill:#ffcccc
    style C fill:#ccffcc
```

#### Implementation:

```bash
# Step 1: Stop Docker service
sudo systemctl stop docker.service
sudo systemctl stop docker.socket

# Verify Docker is stopped
sudo systemctl status docker

# Step 2: Create new directory
sudo mkdir -p /home/temp_docker/mydocker

# Step 3: (Optional) Move existing data
sudo cp -r /var/lib/docker/* /home/temp_docker/mydocker/

# Step 4: Start Docker with new data root
sudo dockerd --data-root /home/temp_docker/mydocker

# Alternative: Edit daemon.json permanently
sudo nano /etc/docker/daemon.json
```

```json
{
  "data-root": "/home/temp_docker/mydocker"
}
```

```bash
# Step 5: Restart Docker service
sudo systemctl daemon-reload
sudo systemctl start docker

# Step 6: Verify new location
docker info | grep "Docker Root Dir"

# Step 7: Verify containers and images
docker ps -a
docker images

# Step 8: Check disk usage
du -sh /home/temp_docker/mydocker
```

**Directory Structure:**

```
/home/temp_docker/mydocker/
├── builder/          # BuildKit cache
├── buildkit/         # BuildKit state
├── containers/       # Container metadata
├── image/            # Image layers
├── network/          # Network configuration
├── overlay2/         # Container layers (overlay2 driver)
├── plugins/          # Plugin data
├── runtimes/         # Runtime configurations
├── swarm/            # Swarm mode data
├── tmp/              # Temporary files
└── volumes/          # Named volumes
```

⚠️ **Important Considerations:**

```
1. Ensure sufficient disk space in new location
2. Backup existing data before moving
3. Update monitoring tools to watch new location
4. Consider filesystem type (ext4, xfs, btrfs)
5. Ensure proper permissions (root:root)
```

**Use Cases:**

- Disk space management
- Using faster storage (SSD vs HDD)
- Separating system and Docker data
- NFS/network storage mounting

---

### Technique 4: Using socat to Monitor Docker API Traffic

**Problem:** You want to debug Docker commands by inspecting API calls.

**Solution:** Use `socat` as a proxy to intercept and log traffic between Docker client and daemon.

```mermaid
sequenceDiagram
    participant Client as Docker Client
    participant Socat as socat Proxy<br/>/tmp/dockerapi.sock
    participant Daemon as Docker Daemon<br/>/var/run/docker.sock
    
    Client->>Socat: docker ps
    Note over Socat: Log Request
    Socat->>Daemon: Forward request
    Daemon-->>Socat: HTTP response
    Note over Socat: Log Response
    Socat-->>Client: Forward response
```

#### Implementation:

```bash
# Step 1: Install socat
sudo apt update && sudo apt install -y socat

# Step 2: Start socat proxy
sudo socat -v \
  UNIX-LISTEN:/tmp/dockerapi.sock,fork \
  UNIX-CONNECT:/var/run/docker.sock &

# Explanation:
# -v                : Verbose mode (log traffic)
# UNIX-LISTEN       : Create listening socket
# fork              : Handle multiple connections
# UNIX-CONNECT      : Connect to Docker daemon

# Step 3: Send commands through proxy
sudo docker -H unix:///tmp/dockerapi.sock ps -a

# Step 4: View intercepted traffic
# Output shows HTTP request/response
```

#### Example Output:

```
> 2024/11/12 10:30:45.123456  length=75 from=0 to=74
GET /v1.41/containers/json?all=1 HTTP/1.1
Host: docker
User-Agent: Docker-Client/20.10.7

< 2024/11/12 10:30:45.234567  length=1024 from=0 to=1023
HTTP/1.1 200 OK
Content-Type: application/json
Date: Tue, 12 Nov 2024 10:30:45 GMT

[{"Id":"abc123","Names":["/mycontainer"],"Image":"nginx",...}]
```

#### Advanced socat Usage:

```bash
# Monitor with timestamp and hex output
sudo socat -v -x \
  UNIX-LISTEN:/tmp/dockerapi.sock,fork \
  UNIX-CONNECT:/var/run/docker.sock

# Save traffic to file
sudo socat -v \
  UNIX-LISTEN:/tmp/dockerapi.sock,fork \
  UNIX-CONNECT:/var/run/docker.sock \
  2>&1 | tee docker-traffic.log

# Monitor TCP traffic (if daemon exposed)
socat -v TCP-LISTEN:2376,fork TCP:localhost:2375
```

#### Understanding Docker API Endpoints:

```bash
# List containers
GET /containers/json

# Inspect container
GET /containers/{id}/json

# Start container
POST /containers/{id}/start

# Stop container
POST /containers/{id}/stop

# Create container
POST /containers/create

# Pull image
POST /images/create?fromImage=nginx&tag=latest

# Build image
POST /build
```

**Use Cases:**

- Debugging Docker client issues
- Understanding Docker API
- Building custom Docker clients
- Troubleshooting network problems
- Learning API endpoints
- Creating API documentation

**Alternative Tools:**

```bash
# tcpdump for network traffic
sudo tcpdump -i lo -A | grep -i docker

# strace for system calls
sudo strace -p $(pidof dockerd) -f

# Wireshark for detailed analysis
wireshark
```

---

### Technique 5: Using Docker in Your Browser

**Problem:** You want to demonstrate Docker without requiring local installation.

**Solution:** Expose Docker daemon with CORS enabled and serve a web-based terminal interface.

```mermaid
graph TB
    Browser[Web Browser] -->|HTTP Requests| Terminal[Docker Terminal UI]
    Terminal -->|Docker API Calls| Daemon[Docker Daemon<br/>Port 2375 + CORS]
    Daemon --> Containers[Containers]
    
    style Browser fill:#e1f5ff
    style Terminal fill:#ffe1e1
    style Daemon fill:#e1ffe1
```

#### Implementation:

This technique extends **Technique 1** (Exposing Docker Daemon) with CORS support.

```bash
# Step 1-6: Same as Technique 1
# Stop Docker, edit systemd service file

# Step 7: Enable CORS in daemon configuration
sudo nano /etc/docker/daemon.json
```

```json
{
  "hosts": ["unix:///var/run/docker.sock", "tcp://0.0.0.0:2375"],
  "api-cors-header": "*"
}
```

```bash
# Alternative: Start dockerd with CORS flag
sudo dockerd -H tcp://0.0.0.0:2375 \
  --api-cors-header="*"

# Step 8: Restart Docker
sudo systemctl daemon-reload
sudo systemctl restart docker

# Step 9: Verify accessibility
curl http://localhost:2375/version
```

#### Web Interface Options:

**1. Portainer (Recommended)**

```bash
# Deploy Portainer
docker run -d \
  -p 9000:9000 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  --name portainer \
  portainer/portainer-ce

# Access: http://localhost:9000
```

**2. Docker Terminal (Lightweight)**

```html
<!-- Simple HTML interface -->
<!DOCTYPE html>
<html>
<head>
    <title>Docker Terminal</title>
</head>
<body>
    <h1>Docker Container Manager</h1>
    <button onclick="listContainers()">List Containers</button>
    <pre id="output"></pre>
    
    <script>
        const DOCKER_HOST = 'http://192.168.1.100:2375';
        
        async function listContainers() {
            try {
                const response = await fetch(`${DOCKER_HOST}/containers/json`);
                const containers = await response.json();
                document.getElementById('output').textContent = 
                    JSON.stringify(containers, null, 2);
            } catch (error) {
                console.error('Error:', error);
            }
        }
    </script>
</body>
</html>
```

**3. Yacht (Modern UI)**

```bash
docker run -d \
  -p 8001:8000 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v yacht-config:/config \
  --name yacht \
  selfhostedpro/yacht
```

#### Security Considerations:

```bash
# ❌ INSECURE - Exposed without authentication
ExecStart=/usr/bin/dockerd -H=tcp://0.0.0.0:2375

# ✅ SECURE - TLS with client certificates
ExecStart=/usr/bin/dockerd \
  --tlsverify \
  --tlscacert=/etc/docker/ca.pem \
  --tlscert=/etc/docker/server-cert.pem \
  --tlskey=/etc/docker/server-key.pem \
  -H=tcp://0.0.0.0:2376

# ✅ SECURE - Behind reverse proxy with auth
# nginx config with basic auth
location /docker/ {
    auth_basic "Docker Access";
    auth_basic_user_file /etc/nginx/.htpasswd;
    proxy_pass http://localhost:2375/;
}
```

**Use Cases:**

- Docker demonstrations
- Training and workshops
- Remote Docker management
- Team collaboration
- Container visualization
- Quick prototyping

⚠️ **Production Warning:**

```
NEVER expose Docker daemon on public internet!
- Full control over host system
- Container escape vulnerabilities
- Data theft possibilities
- Cryptocurrency mining attacks

Always use:
- TLS encryption
- Authentication
- Firewall rules
- VPN access
- Least privilege principle
```

---

### Technique 6: Using Ports to Connect to Containers

**Problem:** You need to make container services accessible from the host machine.

**Solution:** Use Docker's port mapping (`-p` flag) to bind container ports to host ports.

```mermaid
graph LR
    A[Internet] -->|Port 80| B[Host Machine]
    B -->|Port 10001| C[Container 1<br/>WordPress:80]
    B -->|Port 10002| D[Container 2<br/>WordPress:80]
    B -->|Port 10003| E[Container 3<br/>WordPress:80]
    
    style B fill:#ffe1e1
    style C fill:#e1f5ff
    style D fill:#e1ffe1
    style E fill:#fff5e1
```

#### Implementation:

```bash
# Step 1: Pull WordPress image
docker pull wordpress

# Step 2: Run with port mapping
docker run -d \
  -p 10001:80 \
  --name blog1 \
  wordpress

# Port mapping format: HOST_PORT:CONTAINER_PORT
# Host port 10001 maps to container port 80

# Step 3: Verify container is running
docker ps

# Output:
# CONTAINER ID   IMAGE       PORTS                     NAMES
# abc123def456   wordpress   0.0.0.0:10001->80/tcp    blog1

# Step 4: Access via browser
# http://localhost:10001
```

#### Port Mapping Options:

```bash
# 1. Map specific port
docker run -p 8080:80 nginx
# Host:8080 -> Container:80

# 2. Map to random host port
docker run -p 80 nginx
# Docker assigns random host port

# 3. Map specific interface
docker run -p 127.0.0.1:8080:80 nginx
# Only accessible from localhost

# 4. Map multiple ports
docker run \
  -p 80:80 \
  -p 443:443 \
  nginx

# 5. Map UDP ports
docker run -p 53:53/udp dnsmasq

# 6. Map range of ports
docker run -p 8000-8010:8000-8010 myapp

# 7. Publish all exposed ports
docker run -P nginx
# Maps all EXPOSE ports to random host ports
```

#### Real-World Example: Multi-Container WordPress Setup

```bash
# Run MySQL database
docker run -d \
  --name mysql-db \
  -e MYSQL_ROOT_PASSWORD=rootpass \
  -e MYSQL_DATABASE=wordpress \
  -e MYSQL_USER=wpuser \
  -e MYSQL_PASSWORD=wppass \
  -p 3306:3306 \
  mysql:5.7

# Run WordPress (blog1)
docker run -d \
  --name blog1 \
  -e WORDPRESS_DB_HOST=mysql-db:3306 \
  -e WORDPRESS_DB_USER=wpuser \
  -e WORDPRESS_DB_PASSWORD=wppass \
  -e WORDPRESS_DB_NAME=wordpress \
  -p 10001:80 \
  --link mysql-db \
  wordpress

# Run another WordPress instance (blog2)
docker run -d \
  --name blog2 \
  -e WORDPRESS_DB_HOST=mysql-db:3306 \
  -e WORDPRESS_DB_USER=wpuser \
  -e WORDPRESS_DB_PASSWORD=wppass \
  -e WORDPRESS_DB_NAME=wordpress \
  -p 10002:80 \
  --link mysql-db \
  wordpress
```

#### Checking Port Mappings:

```bash
# View port mappings
docker port blog1

# Output:
# 80/tcp -> 0.0.0.0:10001

# Check all container ports
docker ps --format "table {{.Names}}\t{{.Ports}}"

# Find which container uses a port
docker ps --filter "publish=10001"

# Inspect network settings
docker inspect blog1 | grep -A 10 "Ports"
```

#### Port Conflicts and Troubleshooting:

```bash
# Error: Port already in use
# docker: Error response from daemon: driver failed programming 
# external connectivity on endpoint: bind 0.0.0.0:80: address already in use

# Solution 1: Find process using port
sudo lsof -i :80
sudo netstat -tulpn | grep :80

# Solution 2: Stop conflicting service
sudo systemctl stop apache2
sudo systemctl stop nginx

# Solution 3: Use different host port
docker run -p 8080:80 nginx
```

**Use Cases:**

- Web applications (HTTP/HTTPS)
- Databases (MySQL:3306, PostgreSQL:5432)
- Message queues (RabbitMQ:5672, Kafka:9092)
- APIs and microservices
- Development environments
- Load balancer backends

**Best Practices:**

```
✓ Use non-standard ports for security (not 80, 22, etc.)
✓ Document port mappings in docker-compose.yml
✓ Use environment variables for port configuration
✓ Implement health checks on exposed ports
✓ Use reverse proxy (nginx) for production
✓ Limit port exposure to specific interfaces
✓ Monitor port usage and conflicts
```

---

### Technique 7: Allowing Container Communication

**Problem:** Containers need to communicate with each other for microservices architecture.

**Solution:** Create user-defined networks to enable container-to-container communication.

```mermaid
graph TB
    subgraph "User-Defined Network: my_network"
        A[Container 1<br/>Web App] <-->|DNS: container2| B[Container 2<br/>API Server]
        B <-->|DNS: container3| C[Container 3<br/>Database]
        A <-->|DNS: container3| C
    end
    
    subgraph "Bridge Network (Default)"
        D[Container 4] -.X.-> E[Container 5]
    end
    
    style A fill:#e1f5ff
    style B fill:#ffe1e1
    style C fill:#e1ffe1
```

#### Implementation:

```bash
# Step 1: List default networks
docker network ls

# Output:
# NETWORK ID     NAME      DRIVER    SCOPE
# abc123def456   bridge    bridge    local
# def456ghi789   host      host      local
# ghi789jkl012   none      null      local

# Step 2: Create user-defined network
docker network create my_network

# With custom configuration
docker network create \
  --driver bridge \
  --subnet 172.20.0.0/16 \
  --gateway 172.20.0.1 \
  my_custom_network

# Step 3: Run containers on the network
docker run -d \
  --name container1 \
  --network my_network \
  alpine:latest sleep 3600

docker run -d \
  --name container2 \
  --network my_network \
  alpine:latest sleep 3600

# Step 4: Connect existing container to network
docker network connect my_network container3

# Step 5: Inspect network
docker network inspect my_network
```

#### Network Inspection Output:

```json
[
    {
        "Name": "my_network",
        "Driver": "bridge",
        "Scope": "local",
        "IPAM": {
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Containers": {
            "abc123": {
                "Name": "container1",
                "IPv4Address": "172.18.0.2/16"
            },
            "def456": {
                "Name": "container2",
                "IPv4Address": "172.18.0.3/16"
            }
        }
    }
]
```

#### Testing Container Communication:

```bash
# Step 6: Install ping utility
docker exec -it container1 sh
apk add --update iputils
ping container2  # DNS resolution works!

# From host, test connectivity
docker exec container1 ping -c 3 container2

# Output:
# PING container2 (172.18.0.3): 56 data bytes
# 64 bytes from 172.18.0.3: seq=0 ttl=64 time=0.123 ms
# 64 bytes from 172.18.0.3: seq=1 ttl=64 time=0.098 ms
# 64 bytes from 172.18.0.3: seq=2 ttl=64 time=0.102 ms

# Test HTTP communication
docker exec container1 wget -O- http://container2:8000
```

#### Network Drivers:

|Driver|Description|Use Case|
|---|---|---|
|**bridge**|Default, isolated network|Single host, container isolation|
|**host**|Share host network|High performance, no isolation|
|**overlay**|Multi-host network|Docker Swarm, multi-host|
|**macvlan**|Assign MAC address|Legacy apps, direct network access|
|**none**|No networking|Maximum isolation|

#### Advanced Network Configuration:

```bash
# Create overlay network (Swarm mode)
docker network create \
  --driver overlay \
  --subnet 10.0.0.0/24 \
  my_overlay_network

# Create network with custom DNS
docker network create \
  --dns 8.8.8.8 \
  --dns 8.8.4.4 \
  my_dns_network

# Create isolated network
docker network create \
  --internal \
  isolated_network

# Connect container to multiple networks
docker network connect network1 mycontainer
docker network connect network2 mycontainer

# Disconnect from network
docker network disconnect my_network container1

# Remove network
docker network rm my_network

# Prune unused networks
docker network prune
```

#### Real-World Microservices Example:

```bash
# Create application network
docker network create app_network

# Run database
docker run -d \
  --name postgres \
  --network app_network \
  -e POSTGRES_PASSWORD=secret \
  postgres:13

# Run Redis cache
docker run -d \
  --name redis \
  --network app_network \
  redis:alpine

# Run API server
docker run -d \
  --name api \
  --network app_network \
  -e DATABASE_URL=postgresql://postgres:5432/mydb \
  -e REDIS_URL=redis://redis:6379 \
  myapi:latest

# Run frontend
docker run -d \
  --name frontend \
  --network app_network \
  -p 80:80 \
  -e API_URL=http://api:3000 \
  myfrontend:latest
```

#### Network Communication Flow:

```mermaid
sequenceDiagram
    participant Frontend
    participant API
    participant Redis
    participant Postgres
    
    Frontend->>API: HTTP GET /users
    API->>Redis: Check cache
    Redis-->>API: Cache miss
    API->>Postgres: SELECT * FROM users
    Postgres-->>API: User data
    API->>Redis: SET cache
    API-->>Frontend: JSON response
```

**Key Advantages:**

- **DNS Resolution**: Containers can reference each other by name
- **Isolation**: Network segmentation for security
- **Service Discovery**: Automatic DNS-based discovery
- **Load Balancing**: Built-in load balancing (overlay networks)
- **Scalability**: Easy to add/remove containers

**Use Cases:**

- Microservices architecture
- Multi-tier applications (frontend, backend, database)
- Development environments
- Integration testing
- Service mesh implementations

---

### Technique 8: Linking Containers for Port Isolation

**Problem:** Need container communication without creating custom networks (legacy method).

**Solution:** Use Docker's `--link` flag to connect containers.

⚠️ **Note**: Container linking is a legacy feature. **User-defined networks (Technique 7) are preferred** for modern applications.

```mermaid
graph LR
    A[Apache Container] -->|--link mysql| B[MySQL Container]
    A -->|Environment Variables| A
    A -->|/etc/hosts entry| A
    
    style A fill:#e1f5ff
    style B fill:#ffe1e1
```

#### Implementation:

```bash
# Step 1: Check existing networks
docker network ls

# Step 2: Pull required images
docker pull mysql/mysql-server:5.6
docker pull nimmis/apache-php5

# Step 3: Run MySQL container
docker run -d \
  --name mysql \
  -p 3306:3306 \
  -e MYSQL_ROOT_PASSWORD=root123 \
  mysql/mysql-server:5.6

# Step 4: Run Apache with link to MySQL
docker run -d \
  --name apache2 \
  -p 80:80 \
  --link mysql:mysql \
  nimmis/apache-php5

# Link format: --link CONTAINER_NAME:ALIAS
# Creates network connection and DNS entry

# Step 5: Check container status
docker ps

# Step 6: Inspect network configuration
docker inspect mysql | grep -A 20 "Networks"
docker inspect apache2 | grep -A 20 "Networks"

# Step 7: Test connectivity
docker exec -it apache2 bash

# Inside container
ping mysql  # Works!
cat /etc/hosts  # Shows mysql entry
env | grep MYSQL  # Shows environment variables
```

#### How Container Linking Works:

**1. DNS Resolution:**

```bash
# Inside apache2 container
cat /etc/hosts

# Output:
# 172.17.0.3      apache2
# 172.17.0.2      mysql abc123def456
```

**2. Environment Variables:**

```bash
# Inside apache2 container
env | grep MYSQL

# Output:
# MYSQL_PORT=tcp://172.17.0.2:3306
# MYSQL_PORT_3306_TCP=tcp://172.17.0.2:3306
# MYSQL_PORT_3306_TCP_ADDR=172.17.0.2
# MYSQL_PORT_3306_TCP_PORT=3306
# MYSQL_PORT_3306_TCP_PROTO=tcp
# MYSQL_NAME=/apache2/mysql
```

**3. Network Connection:**

```bash
# Test MySQL connection from Apache
docker exec -it apache2 bash

# Install MySQL client
apt-get update && apt-get install -y mysql-client

# Connect to MySQL
mysql -h mysql -u root -proot123

# Output:
# Welcome to the MySQL monitor...
# mysql>
```

#### Complete WordPress + MySQL Example:

```bash
# Step 1: Create MySQL container
docker run -d \
  --name wordpress-mysql \
  -e MYSQL_ROOT_PASSWORD=rootpass \
  -e MYSQL_DATABASE=wordpress \
  -e MYSQL_USER=wpuser \
  -e MYSQL_PASSWORD=wppass \
  mysql:5.7

# Step 2: Create WordPress container with link
docker run -d \
  --name wordpress-app \
  --link wordpress-mysql:mysql \
  -p 8080:80 \
  -e WORDPRESS_DB_HOST=mysql:3306 \
  -e WORDPRESS_DB_USER=wpuser \
  -e WORDPRESS_DB_PASSWORD=wppass \
  -e WORDPRESS_DB_NAME=wordpress \
  wordpress:latest

# Step 3: Access WordPress
# http://localhost:8080
```

#### Linking vs User-Defined Networks:

|Feature|Container Linking|User-Defined Networks|
|---|---|---|
|DNS Resolution|✓ (one-way)|✓ (bidirectional)|
|Environment Variables|✓|✗|
|Dynamic Discovery|✗|✓|
|Multiple Networks|✗|✓|
|Container Restart|Breaks links|Maintains connectivity|
|Modern Practice|✗ (legacy)|✓ (recommended)|

#### Link Direction:

```mermaid
graph LR
    A[Container A<br/>--link B] -->|Can connect to| B[Container B]
    B -.X.->|Cannot connect to| A
    
    style A fill:#e1f5ff
    style B fill:#ffe1e1
```

**Important Limitations:**

```
❌ One-way communication (A can reach B, but B cannot reach A)
❌ Cannot link to containers on different networks
❌ Links break if linked container restarts
❌ No automatic service discovery
❌ Deprecated in favor of networks
```

#### Migration to Networks:

```bash
# OLD WAY (linking)
docker run --name db mysql
docker run --name app --link db:database myapp

# NEW WAY (networks)
docker network create myapp_network
docker run --name db --network myapp_network mysql
docker run --name app --network myapp_network myapp
# Both can communicate bidirectionally
```

**Use Cases (Historical):**

- Legacy applications
- Simple dev environments
- Learning Docker basics
- Backward compatibility

**Modern Alternatives:**

- User-defined networks (Technique 7)
- Docker Compose
- Kubernetes Services
- Service mesh (Istio, Linkerd)

---

### Technique 9: Setting Up a Local Docker Registry

**Problem:** You need a private registry to host your images locally or within your organization.

**Solution:** Deploy the official Docker Registry container.

```mermaid
graph TB
    Dev[Developer] -->|docker push| Registry[Local Registry<br/>localhost:5000]
    Registry -->|docker pull| Prod[Production Server]
    Registry -->|docker pull| Test[Test Server]
    Registry <-->|Storage| Volume[Host Volume<br/>$HOME/registry]
    
    style Registry fill:#ffe1e1
    style Volume fill:#e1ffe1
```

#### Implementation:

```bash
# Step 1: Run registry container
docker run -d \
  -p 5000:5000 \
  -v $HOME/registry:/var/lib/registry \
  --name local-registry \
  --restart=always \
  registry:2

# Breakdown:
# -p 5000:5000     : Expose on port 5000
# -v $HOME/registry: Persist images on host
# --restart=always : Auto-restart on failures

# Step 2: Verify registry is running
docker ps

# Step 3: Check registry API
curl http://localhost:5000/v2/_catalog

# Output:
# {"repositories":[]}

# Step 4: Tag image for local registry
docker tag alpine:latest localhost:5000/alpine:latest

# Step 5: Push image to local registry
docker push localhost:5000/alpine:latest

# Step 6: Verify image in registry
curl http://localhost:5000/v2/_catalog

# Output:
# {"repositories":["alpine"]}

# Step 7: Remove local image
docker rmi localhost:5000/alpine:latest

# Step 8: Pull from local registry
docker pull localhost:5000/alpine:latest
```

#### Advanced Registry Configuration:

**1. With Authentication:**

```bash
# Create password file
mkdir -p ~/registry/auth
docker run --rm \
  --entrypoint htpasswd \
  httpd:2 -Bbn admin password123 > ~/registry/auth/htpasswd

# Run registry with authentication
docker run -d \
  -p 5000:5000 \
  -v $HOME/registry/auth:/auth \
  -v $HOME/registry/data:/var/lib/registry \
  -e "REGISTRY_AUTH=htpasswd" \
  -e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" \
  -e "REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd" \
  --name secure-registry \
  registry:2

# Login to registry
docker login localhost:5000
# Username: admin
# Password: password123
```

**2. With TLS/HTTPS:**

```bash
# Generate self-signed certificate
mkdir -p ~/registry/certs
openssl req -newkey rsa:4096 -nodes \
  -sha256 -keyout ~/registry/certs/domain.key \
  -x509 -days 365 -out ~/registry/certs/domain.crt

# Run registry with TLS
docker run -d \
  -p 5000:5000 \
  -v $HOME/registry/certs:/certs \
  -v $HOME/registry/data:/var/lib/registry \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
  --name tls-registry \
  registry:2
```

**3. With Storage Backends:**

```bash
# AWS S3 backend
docker run -d \
  -p 5000:5000 \
  -e REGISTRY_STORAGE=s3 \
  -e REGISTRY_STORAGE_S3_ACCESSKEY=AKIAIOSFODNN7EXAMPLE \
  -e REGISTRY_STORAGE_S3_SECRETKEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY \
  -e REGISTRY_STORAGE_S3_REGION=us-east-1 \
  -e REGISTRY_STORAGE_S3_BUCKET=my-registry \
  registry:2

# Azure Blob Storage
docker run -d \
  -p 5000:5000 \
  -e REGISTRY_STORAGE=azure \
  -e REGISTRY_STORAGE_AZURE_ACCOUNTNAME=myaccount \
  -e REGISTRY_STORAGE_AZURE_ACCOUNTKEY=mykey \
  -e REGISTRY_STORAGE_AZURE_CONTAINER=registry \
  registry:2
```

#### Registry API Operations:

```bash
# List all repositories
curl http://localhost:5000/v2/_catalog

# List tags for repository
curl http://localhost:5000/v2/alpine/tags/list

# Get image manifest
curl http://localhost:5000/v2/alpine/manifests/latest

# Delete image (enable deletion first)
curl -X DELETE http://localhost:5000/v2/alpine/manifests/sha256:abc123...

# Check registry health
curl http://localhost:5000/v2/

# Get registry configuration
docker exec local-registry cat /etc/docker/registry/config.yml
```

#### Working with Local Registry:

```bash
# Build and push workflow
docker build -t myapp:v1 .
docker tag myapp:v1 localhost:5000/myapp:v1
docker push localhost:5000/myapp:v1

# Pull workflow
docker pull localhost:5000/myapp:v1
docker run localhost:5000/myapp:v1

# Copy image between registries
docker pull alpine:latest
docker tag alpine:latest localhost:5000/alpine:latest
docker push localhost:5000/alpine:latest
```

#### Registry Maintenance:

```bash
# View registry storage
du -sh $HOME/registry

# Garbage collection (remove deleted images)
docker exec local-registry bin/registry garbage-collect \
  /etc/docker/registry/config.yml

# Backup registry
tar -czf registry-backup.tar.gz $HOME/registry

# Restore registry
tar -xzf registry-backup.tar.gz -C $HOME/
```

#### Docker Compose Configuration:

```yaml
version: '3.8'

services:
  registry:
    image: registry:2
    ports:
      - "5000:5000"
    environment:
      REGISTRY_STORAGE_FILESYSTEM_ROOTDIRECTORY: /data
      REGISTRY_AUTH: htpasswd
      REGISTRY_AUTH_HTPASSWD_PATH: /auth/htpasswd
      REGISTRY_AUTH_HTPASSWD_REALM: Registry Realm
    volumes:
      - ./registry-data:/data
      - ./registry-auth:/auth
    restart: always

  registry-ui:
    image: joxit/docker-registry-ui:latest
    ports:
      - "8080:80"
    environment:
      REGISTRY_TITLE: My Docker Registry
      REGISTRY_URL: http://registry:5000
      SINGLE_REGISTRY: true
    depends_on:
      - registry
```

**Use Cases:**

- Private image storage
- CI/CD pipelines
- Air-gapped environments
- Faster image distribution (local network)
- Image version control
- Organizational image hub
- Development/testing environments

**Production Alternatives:**

- **Harbor**: Enterprise-grade registry with UI, security scanning
- **JFrog Artifactory**: Multi-format artifact repository
- **Cloud Registries**: Docker Hub, AWS ECR, Google GCR, Azure ACR
- **GitLab Container Registry**: Integrated with GitLab CI/CD
- **Quay.io**: Red Hat's enterprise registry

---

### Technique 10: Finding and Running a Docker Image

**Problem:** You want to discover and run existing Docker images without building from scratch.

**Solution:** Use Docker Hub search and pull functionality.

```mermaid
graph LR
    A[Search Docker Hub] --> B{Found Image?}
    B -->|Yes| C[Pull Image]
    B -->|No| D[Build Custom Image]
    C --> E[Run Container]
    D --> E
    
    style A fill:#e1f5ff
    style C fill:#e1ffe1
    style E fill:#ffe1e1
```

#### Implementation:

```bash
# Step 1: Search for images
docker search alpine

# Output:
# NAME                 DESCRIPTION           STARS     OFFICIAL
# alpine               A minimal Docker...   8500      [OK]
# alpine/git           A simple git...       200       
# alpine/helm          Auto-trigger...       50        

# Search with filters
docker search --filter stars=100 alpine
docker search --filter is-official=true nginx

# Limit results
docker search --limit 5 python

# Step 2: Pull the image
docker pull alpine:latest

# Pull specific version
docker pull alpine:3.18

# Pull from specific registry
docker pull gcr.io/google-containers/nginx:latest

# Step 3: Verify downloaded image
docker images

# Step 4: Run the image
docker run -it alpine:latest /bin/sh

# Run with specific command
docker run alpine:latest echo "Hello Docker"

# Run in detached mode
docker run -d alpine:latest sleep 3600
```

#### Exploring Docker Hub:

**1. Web Interface:**

- Visit: https://hub.docker.com
- Search for images
- View documentation
- Check tags and versions
- Read reviews and ratings

**2. Official vs Community Images:**

|Type|Description|Trust Level|Examples|
|---|---|---|---|
|**Official Images**|Verified by Docker|High|nginx, mysql, redis, python|
|**Verified Publisher**|From trusted organizations|Medium-High|microsoft/dotnet, oracle/mysql|
|**Community Images**|User-created|Variable|user/myapp|

#### Common Official Images:

```bash
# Web Servers
docker pull nginx                    # Nginx web server
docker pull httpd                    # Apache HTTP Server
docker pull caddy                    # Caddy web server

# Programming Languages
docker pull node:18-alpine           # Node.js
docker pull python:3.11-slim         # Python
docker pull openjdk:17-jdk           # Java
docker pull golang:1.21              # Go
docker pull rust:latest              # Rust

# Databases
docker pull postgres:15              # PostgreSQL
docker pull mysql:8                  # MySQL
docker pull mongo:7                  # MongoDB
docker pull redis:7-alpine           # Redis
docker pull mariadb:11               # MariaDB

# Message Queues
docker pull rabbitmq:3-management    # RabbitMQ
docker pull confluentinc/cp-kafka    # Apache Kafka
docker pull nats:latest              # NATS

# Tools
docker pull jenkins/jenkins:lts      # Jenkins CI/CD
docker pull sonarqube:community      # SonarQube
docker pull gitlab/gitlab-ce         # GitLab
```

#### Understanding Image Tags:

```bash
# Anatomy of image name
[registry/][username/]repository:tag

# Examples:
alpine:latest              # Default registry (Docker Hub), latest tag
alpine:3.18                # Specific version
node:18-alpine             # Node 18 on Alpine base
myregistry.com/myapp:v1    # Private registry
username/myapp:dev         # User repository
```

#### Inspecting Images:

```bash
# View image details
docker inspect alpine:latest

# View image layers
docker history alpine:latest

# Output:
# IMAGE          CREATED        CREATED BY                     SIZE
# abc123def456   2 weeks ago    /bin/sh -c #(nop)  CMD...     0B
# def456ghi789   2 weeks ago    /bin/sh -c #(nop) ADD...      7.05MB

# Check image size
docker images alpine

# View image metadata
docker inspect alpine:latest | jq '.[0].Config'
```

#### Running Popular Images:

**1. Web Server (Nginx):**

```bash
# Pull and run nginx
docker pull nginx:alpine
docker run -d \
  --name webserver \
  -p 8080:80 \
  -v $(pwd)/html:/usr/share/nginx/html:ro \
  nginx:alpine

# Create simple HTML
echo "<h1>Hello from Docker!</h1>" > html/index.html

# Access: http://localhost:8080
```

**2. Database (PostgreSQL):**

```bash
# Pull and run PostgreSQL
docker pull postgres:15
docker run -d \
  --name postgres-db \
  -e POSTGRES_PASSWORD=mysecretpass \
  -e POSTGRES_DB=mydb \
  -p 5432:5432 \
  -v pgdata:/var/lib/postgresql/data \
  postgres:15

# Connect to database
docker exec -it postgres-db psql -U postgres
```

**3. Development Environment (Python):**

```bash
# Pull Python image
docker pull python:3.11-slim

# Run interactive Python shell
docker run -it python:3.11-slim python

# Run Python script
docker run -v $(pwd):/app -w /app python:3.11-slim python script.py
```

**4. Message Queue (RabbitMQ):**

```bash
# Pull RabbitMQ with management UI
docker pull rabbitmq:3-management

# Run RabbitMQ
docker run -d \
  --name rabbitmq \
  -p 5672:5672 \
  -p 15672:15672 \
  rabbitmq:3-management

# Access management UI: http://localhost:15672
# Default credentials: guest/guest
```

#### Advanced Search Techniques:

```bash
# Search with multiple filters
docker search \
  --filter stars=50 \
  --filter is-official=true \
  --limit 10 \
  database

# Search using API
curl -s "https://registry.hub.docker.com/v2/repositories/library/alpine/tags/" | \
  jq -r '.results[].name'

# Find image digest
docker pull alpine:latest
docker images --digests alpine

# Pull by digest (immutable)
docker pull alpine@sha256:abc123def456...
```

#### Image Verification:

```bash
# Enable Docker Content Trust
export DOCKER_CONTENT_TRUST=1

# Pull verified images only
docker pull alpine:latest
# Verifies signatures before pulling

# View image signatures
docker trust inspect alpine:latest

# Disable content trust
export DOCKER_CONTENT_TRUST=0
```

#### Creating Image from Container:

```bash
# Run base image
docker run -it --name custom alpine:latest sh

# Inside container, make changes
apk add --update nodejs npm
exit

# Commit changes to new image
docker commit custom myalpine:nodejs

# View new image
docker images myalpine

# Run new image
docker run -it myalpine:nodejs node --version
```

#### Docker Hub Alternatives:

|Registry|URL|Features|
|---|---|---|
|**Quay.io**|quay.io|Red Hat, security scanning|
|**GitHub Container Registry**|ghcr.io|Integrated with GitHub|
|**GitLab Container Registry**|registry.gitlab.com|Integrated with GitLab CI|
|**Amazon ECR**|AWS|AWS integration, private only|
|**Google GCR**|gcr.io|Google Cloud integration|
|**Azure ACR**|azurecr.io|Azure integration|

**Best Practices:**

```
✓ Always use specific tags (not 'latest') in production
✓ Prefer official images over community images
✓ Check image size (prefer slim/alpine variants)
✓ Review image security vulnerabilities
✓ Read image documentation on Docker Hub
✓ Check last updated date
✓ Review Dockerfile on GitHub
✓ Scan images for vulnerabilities (docker scan)
```

**Security Scanning:**

```bash
# Scan image for vulnerabilities (requires Docker Hub login)
docker scan alpine:latest

# Scan with Trivy (open-source)
docker run --rm \
  -v /var/run/docker.sock:/var/run/docker.sock \
  aquasec/trivy:latest image alpine:latest

# Scan with Clair
docker run --rm \
  -v /var/run/docker.sock:/var/run/docker.sock \
  arminc/clair-scanner:latest --ip localhost alpine:latest
```

**Use Cases:**

- Rapid prototyping
- Development environments
- CI/CD pipelines
- Learning new technologies
- Microservices deployment
- Quick testing
- Production deployments

---

## Summary & Key Takeaways

### DevOps Culture

```mermaid
mindmap
  root((DevOps))
    Automation
      CI/CD Pipelines
      Infrastructure as Code
      Automated Testing
    Collaboration
      Dev + Ops Unity
      Shared Responsibility
      Cross-functional Teams
    Monitoring
      Continuous Feedback
      Performance Metrics
      Log Aggregation
    Culture
      Fail Fast
      Continuous Learning
      Blameless Postmortems
```

### Docker Core Concepts

**1. Containers vs VMs:**

- Containers: Share OS kernel, lightweight, fast startup
- VMs: Full OS per instance, heavy, slow startup
- Use containers for microservices, VMs for complete isolation

**2. Image Layers:**

- Images built in layers
- Each Dockerfile instruction = new layer
- Layers are cached and reusable
- Optimize by ordering instructions (least to most frequently changed)

**3. Docker Architecture:**

- Client-Server model
- Docker daemon manages containers
- REST API communication
- Registries store images

### 10 Essential Techniques Summary

|Technique|Purpose|Key Command|Use Case|
|---|---|---|---|
|**1. TCP Daemon**|Remote Docker access|`dockerd -H tcp://0.0.0.0:2375`|CI/CD, remote management|
|**2. Daemon Containers**|Background services|`docker run -d`|Web servers, databases|
|**3. Data Root**|Change storage location|`dockerd --data-root /path`|Disk space management|
|**4. socat Monitor**|Debug API calls|`socat -v UNIX-LISTEN`|Troubleshooting, learning|
|**5. Browser UI**|Web-based management|Portainer, Yacht|Demos, visualization|
|**6. Port Mapping**|Expose services|`docker run -p HOST:CONTAINER`|Web apps, APIs|
|**7. Networks**|Container communication|`docker network create`|Microservices|
|**8. Linking**|Legacy communication|`docker run --link`|Old applications|
|**9. Local Registry**|Private image storage|`docker run registry:2`|Organizations|
|**10. Find & Run**|Discover images|`docker search, docker pull`|Quick deployment|

### Best Practices Checklist

#### Security:

- [ ] Never expose Docker daemon without TLS
- [ ] Use non-root users in containers
- [ ] Scan images for vulnerabilities
- [ ] Use specific image tags (not 'latest')
- [ ] Keep base images updated
- [ ] Limit container resources
- [ ] Use secrets management (Docker secrets, Vault)

#### Performance:

- [ ] Use alpine/slim base images
- [ ] Minimize layer count
- [ ] Use multi-stage builds
- [ ] Implement health checks
- [ ] Monitor resource usage
- [ ] Use appropriate restart policies

#### Development:

- [ ] Use .dockerignore file
- [ ] Document Dockerfiles
- [ ] Version control Dockerfiles
- [ ] Use docker-compose for multi-container apps
- [ ] Test images before deployment
- [ ] Implement CI/CD pipelines

#### Operations:

- [ ] Implement logging strategy
- [ ] Use volume mounts for persistent data
- [ ] Regular backup of volumes
- [ ] Monitor container health
- [ ] Implement rollback strategy
- [ ] Document network architecture

### Common Docker Commands Reference

```bash
# Image Management
docker build -t name:tag .          # Build image
docker images                       # List images
docker pull image:tag               # Pull image
docker push image:tag               # Push image
docker rmi image:tag                # Remove image
docker tag source target            # Tag image
docker history image:tag            # View layers

# Container Management
docker run [options] image          # Create & start
docker ps                           # List running
docker ps -a                        # List all
docker start container              # Start stopped
docker stop container               # Stop running
docker restart container            # Restart
docker rm container                 # Remove
docker exec -it container cmd       # Execute command
docker logs [-f] container          # View logs
docker inspect container            # View details
docker stats                        # View resource usage

# Network Management
docker network ls                   # List networks
docker network create name          # Create network
docker network connect net cont     # Connect container
docker network inspect name         # View details
docker network rm name              # Remove network

# Volume Management
docker volume ls                    # List volumes
docker volume create name           # Create volume
docker volume inspect name          # View details
docker volume rm name               # Remove volume

# Registry Operations
docker login [registry]             # Login
docker logout [registry]            # Logout
docker search term                  # Search images
docker pull registry/image:tag      # Pull from registry
docker push registry/image:tag      # Push to registry

# System Management
docker info                         # System info
docker version                      # Docker version
docker system df                    # Disk usage
docker system prune                 # Clean up
docker system prune -a              # Clean all unused
```

### Exam Preparation Tips

**Theory Questions:**

1. Explain DevOps lifecycle and its stages
2. Compare containers vs virtual machines
3. Describe Docker architecture components
4. Explain image layers and caching
5. Differentiate user-defined networks vs bridge networks
6. Security considerations for Docker daemon
7. Registry types and use cases

**Practical Questions:**

1. Write Dockerfile for a web application
2. Create multi-container setup with networking
3. Implement persistent storage with volumes
4. Configure Docker daemon for remote access
5. Set up local Docker registry
6. Debug container networking issues
7. Optimize Dockerfile for smaller images

**Diagram Questions:**

- Draw Docker architecture
- Illustrate DevOps lifecycle
- Show container networking
- Explain image layers
- Docker registry flow

### Additional Resources

**Official Documentation:**

- Docker Docs: https://docs.docker.com
- Docker Hub: https://hub.docker.com
- Docker Compose: https://docs.docker.com/compose
- Docker Networking: https://docs.docker.com/network

**Learning Platforms:**

- Play with Docker: https://labs.play-with-docker.com
- Docker Training: https://training.docker.com
- Katacoda Docker Scenarios: https://katacoda.com/courses/docker

**Tools:**

- Portainer: Container management UI
- Dive: Analyze image layers
- Trivy: Security scanner
- Docker Bench: Security audit
- Hadolint: Dockerfile linter

---

## Appendix: Docker Compose Example

For managing multi-container applications, Docker Compose is essential:

```yaml
version: '3.8'

services:
  # Web Application
  web:
    build: ./web
    ports:
      - "80:8000"
    environment:
      - DATABASE_URL=postgresql://db:5432/myapp
      - REDIS_URL=redis://cache:6379
    depends_on:
      - db
      - cache
    networks:
      - app-network
    volumes:
      - ./web:/app
    restart: unless-stopped

  # Database
  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_DB: myapp
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - pgdata:/var/lib/postgresql/data
    networks:
      - app-network
    restart: unless-stopped

  # Cache
  cache:
    image: redis:7-alpine
    networks:
      - app-network
    restart: unless-stopped

networks:
  app-network:
    driver: bridge

volumes:
  pgdata:
```

**Commands:**

```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop all services
docker-compose down

# Rebuild and restart
docker-compose up -d --build

# Scale service
docker-compose up -d --scale web=3
```

---

## End of Notes

**Document Version:** 1.0  
**Last Updated:** November 12, 2024  
**Topics Covered:** DevOps Culture, Docker Fundamentals, 10 Essential Techniques  
**Suitable For:** College exams, certification prep, practical learning

**Good luck with your studies! 🚀**