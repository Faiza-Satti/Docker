# Docker Notes

A complete beginner-to-advanced guide on Docker.  
This repository is meant to help anyone learn Docker from scratch and grow towards advanced concepts.

---

## 📖 Table of Contents
1. [Introduction to Containers](#introduction-to-containers)
2. [Why Docker?](#why-docker)
3. [Installing Docker](#installing-docker)
4. [Basic Docker Commands](#basic-docker-commands)
5. [Working with Images](#working-with-images)
6. [Working with Containers](#working-with-containers)
7. [Docker Volumes](#docker-volumes)
8. [Docker Networks](#docker-networks)
9. [Docker Compose](#docker-compose)
10. [Dockerfile](#dockerfile)
11. [Best Practices](#best-practices)
12. [Debugging & Logs](#debugging--logs)
13. [Real-world Use Cases](#real-world-use-cases)
14. [Docker Swarm](#docker-swarm)
15. [Kubernetes Intro](#kubernetes-intro)
16. [Security & Optimization](#security--optimization)
17. [Resources & Learning](#resources--learning)

---

## 1. Introduction to Containers
- A **container** is a lightweight, portable unit that packages code, runtime, system tools, and libraries.  
- Think of it as a **virtual environment**, but faster and isolated.  
- Containers run on the **same OS kernel** but are isolated from each other.

---

## 2. Why Docker?
- Consistency: "Works on my machine" problem solved.  
- Portability: Run apps anywhere (Windows, Linux, Mac, cloud).  
- Efficiency: Uses fewer resources than VMs.  
- Scalability: Easy to scale up microservices.

---

## 3. Installing Docker
- [Download Docker Desktop](https://www.docker.com/products/docker-desktop) for Windows/Mac.  
- For Linux:
  ```bash
  sudo apt update
  sudo apt install docker.io -y
  sudo systemctl start docker
  sudo systemctl enable docker

## 4. Basic Docker Commands

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

## 5. Working with Images
```bash
# Pull image
docker pull nginx

# Build from Dockerfile
docker build -t myapp:1.0 .

# Remove image
docker rmi <image-id>
```

## 6. Working with Containers
```bash
# Run container
docker run -d -p 8080:80 nginx

# Attach to a running container
docker exec -it <id> bash

# View logs
docker logs <id>
```

## 7. Docker Volumes
```bash
docker volume create mydata
docker run -v mydata:/data busybox
```

## 8. Docker Networks
```bash
docker network create mynet
docker run -d --network=mynet --name=db mysql
docker run -d --network=mynet --name=webapp nginx
```

## 9. Docker Compose
```bash
version: '3'
services:
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: root
  web:
    image: nginx
    ports:
      - "8080:80"
```

## 10. Dockerfile
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

## 11. Best Practices
- Use .dockerignore to reduce image size.
- Use multi-stage builds.
- Keep containers small and focused.
- Don’t store secrets inside images.

## 12. Debugging & Logs
docker logs <container>
docker inspect <container>
docker exec -it <container> sh

## 13. Real-world Use Cases
- Running databases locally (MySQL, MongoDB).
- Setting up CI/CD pipelines.
- Microservices deployments.
- Local dev environments.

## 14. Docker Swarm
docker swarm init
docker service create --replicas 3 nginx

## 15. Kubernetes Intro
- Kubernetes (K8s) is the industry standard for orchestration.
- Docker containers run inside Pods.
- Tools like kubectl manage deployments.

## 16. Security & Optimization
- Always use official images.
- Run as non-root user.
- Scan images for vulnerabilities (docker scan).
- Keep images minimal (e.g., alpine).

## 17. Resources & Learning
- Docker Docs
- YouTube: TechWorld with Nana (Docker/K8s tutorials)
- Books: Docker Deep Dive by Nigel Poulton
