# Docker Notes

These are notes I made while learning from various resources. Feel free to correct me or provide feedback. I believe it's never too late to start again as a beginner. 🙂

---

## Table of Contents
1. [Introduction to Containers](#1-introduction-to-containers)
2. [Why Docker?](#2-why-docker)
3. [Docker Components](#3-docker-components)
4. [Installing Docker](#4-installing-docker)
5. [Basic Docker Commands](#5-basic-docker-commands)
6. [Working with Images](#6-working-with-images)
7. [Working with Containers](#7-working-with-containers)
8. [Docker Image Tags](#8-docker-image-tags)
9. [Docker Volumes](#9-docker-volumes)
10. [Docker Networks](#10-docker-networks)
11. [Docker Compose](#11-docker-compose)
12. [Dockerfile](#12-dockerfile)
13. [Best Practices](#13-best-practices)
14. [Debugging & Logs](#14-debugging--logs)
15. [Real-world Use Cases](#15-real-world-use-cases)
16. [Security & Optimization](#16-security--optimization)
---

## 1. Introduction to Containers
- A **container** is a lightweight, standalone, and executable software package that includes everything needed to run a piece of software.
- Think of it as a **virtual environment**, but faster and isolated.  
- Containers run on the **same OS kernel** but are isolated from each other.

---

## 2. Why Docker?
Docker is a platform designed to simplify the process of building, deploying, and running applications using containerization. It packages software and its dependencies into standardized units called containers.
- Docker uses host's OS and memory system, making it leighweight.
- Docker image sizes are that's why in MBs.

### What did devs use before docker?
- Manual Server Setup and Configuration
- Virtual Machines (VMs)
- Shell Scripts for Deployment

---

## 3. Docker Components
Docker is built around a few key components that work together to enable containerization:

---

### 1. Docker Engine
- **What it is:** The **core runtime environment** that makes Docker work.  
- **How it works:** It provides the client-server architecture where:
  - **Docker CLI (Client):** Runs commands like `docker run`, `docker build`.  
  - **Docker Daemon (Server):** Listens for commands from the client and manages container lifecycle.  
- **Purpose:** The “brain” of Docker that makes containerization possible.

---

### 2. Docker Images
- **What it is:** A **read-only template** that contains everything needed to run an application (source code, runtime, libraries, environment variables, configs).  
- **Layers:** Images are built in layers (using `Dockerfile` instructions like `FROM`, `RUN`, `COPY`).  
- **Immutable:** Once built, an image doesn’t change. If you need changes, you create a new image version.  
- **Usage:** Containers are created **from images**.  

Example:
```bash
docker build -t myapp:1.0 .
docker run myapp:1.0
```

### 3. Docker Daemon (dockerd)
- **What it is:** A background service that runs on the host machine.
- Responsibilities:
  - Builds, runs, and distributes Docker containers.
  - Talks to the Docker CLI.
  - Manages images, networks, and volumes.
- Communication: The CLI communicates with the daemon using REST API calls over a Unix socket (/var/run/docker.sock) or TCP.

### 4. Docker Registry
- **What it is:** A storage system for Docker images.
- Types:
  - Public Registry: Docker Hub (default registry).
  - Private Registries: Azure Container Registry (ACR), AWS ECR, Harbor, GitHub Container Registry, etc.
- Workflow:
  - Push: Upload your built images (docker push myapp:1.0).
  - Pull: Download images when running containers (docker pull mysql:1.25).
- Benefit: Makes sharing and reusing images easy across teams and environments.

### Putting it Together
- You write a Dockerfile → build an image.
- Image is stored locally or in a registry.
- You use Docker CLI to tell Docker Engine to start a container.
- Docker Daemon runs that container using the image.

---

## 4. Installing Docker
- [Download Docker Desktop](https://www.docker.com/products/docker-desktop) for Windows/Mac.  
- For Linux:
  ```bash
  sudo apt update
  sudo apt install docker.io -y
  sudo systemctl start docker
  sudo systemctl enable docker

## One Dockerfile, Multiple Containers
- A single Dockerfile defines how an image is built (dependencies, environment, commands).
- Once built, you can create as many containers as you want from that same image.
```bash
# Build an image from Dockerfile
docker build -t myapp:1.0 .

# Run multiple containers from the same image
docker run -d -p 3000:3000 --name app1 myapp:1.0
docker run -d -p 3001:3000 --name app2 myapp:1.0
docker run -d -p 3002:3000 --name app3 myapp:1.0
```

- myapp:1.0 is one image (built from one Dockerfile).
- app1, app2, app3 are three containers running independently.
- Containers share the same base image but run in isolated environments.

---

## 5. Basic Docker Commands
```bash
# Check Docker version
docker --version

# Run hello-world test container
docker run hello-world

# List containers
docker ps
docker ps -a

# Stop and remove container
docker stop <container-id>
docker rm <container-id>

# List images
docker images
```
---

## 6. Working with Images
```bash
# Pull image
docker pull nginx

# Build from Dockerfile
docker build -t myapp:1.0 .

# Remove image
docker rmi <image-id>
```
---

## 7. Working with Containers
```bash
# Run container
docker run -d -p 8080:80 nginx

# Attach to a running container
docker exec -it <id> bash

# View logs
docker logs <id>
```
---

## 8. Docker Image Tags
Docker images are versioned and identified using **tags**. By default, if you don’t specify a tag, Docker uses `:latest`.

### Examples:
```bash
# Pull the latest nginx image
docker pull nginx

# Pull a specific version of nginx
docker pull nginx:1.25

# Run a container from a tagged image
docker run -d nginx:1.25

# Tag a local image before pushing
docker tag myapp:1.0 faizi/myapp:1.0
```
### Remote Tags
- Docker images live in registries (like Docker Hub, AWS ECR, GCP Artifact Registry).
- A remote tag usually includes the registry, namespace (username/org), image name, and tag.

### Example
```bash
# Pull from Docker Hub (default registry)
docker pull library/redis:7
#Good practice: Always use explicit tags (like 1.25, 2.0.1) instead of latest for production.
```
---

## 9. Docker Volumes
```bash
docker volume create mydata
docker run -v mydata:/data busybox
```
---

## 10. Docker Networks
```bash
docker network create mynet
docker run -d --network=mynet --name=db mysql
docker run -d --network=mynet --name=webapp nginx
```
---

## 11. Docker Compose
**Docker Compose** is a tool that helps you define and run multi-container Docker applications. Instead of starting containers manually with long `docker run` commands, you describe your app’s services (databases, backends, frontends, etc.) in a **`docker-compose.yml`** file and run them with a single command.

### Key Features
- **Multi-container management**: Define multiple services (e.g., web app + database + cache) in one YAML file.  
- **Service isolation**: Each service runs in its own container, but they can easily communicate via Docker’s network.  
- **Declarative syntax**: Everything is defined in `docker-compose.yml`, making it reproducible and version-controlled.  
- **Environment support**: Use `.env` files for configuration and secrets.  
- **Scaling**: You can scale services (e.g., multiple web instances) using `docker-compose up --scale`.  

### Common Commands
- `docker-compose up` → Start all services (build images if needed).  
- `docker-compose up -d` → Start in detached (background) mode.  
- `docker-compose down` → Stop and remove containers, networks, volumes.  
- `docker-compose build` → Build/rebuild services.  
- `docker-compose logs -f` → Stream logs from all services.  
- `docker-compose ps` → List running services.  

### Example `docker-compose.yml`
```yaml
version: "3.8"

services:
  app:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/app
    depends_on:
      - db

  db:
    image: postgres:14
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: mydb
    volumes:
      - db_data:/var/lib/postgresql/data

volumes:
  db_data:
```
---

## 12. Dockerfile
```bash
# Use base image
FROM node:18

# Set working directory
WORKDIR /app

# Copy files
COPY package*.json ./
RUN npm install
COPY . .

# Expose port
EXPOSE 3000

# Start app
CMD ["npm", "start"]
```
---

## 13. Best Practices
- Use .dockerignore to reduce image size.
- Use multi-stage builds.
- Keep containers small and focused.
- Don’t store secrets inside images.

---

## 14. Debugging & Logs
```bash
docker logs <container>
docker inspect <container>
docker exec -it <container> sh
```

---

## 15. Real-world Use Cases
- Running databases locally (MySQL, MongoDB).
- Setting up CI/CD pipelines.
- Microservices deployments.
- Local dev environments.

---

## 16. Security & Optimization
- Always use official images.
- Run as non-root user.
- Scan images for vulnerabilities (docker scan).
- Keep images minimal.

I recommend a book as a docker learning resource : **Docker Deep Dive by Nigel Poulton**.
Never stop learning ✨
