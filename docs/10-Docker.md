# Docker

> *"Docker is an open-source containerization platform that enables developers to package applications and their dependencies into lightweight, portable, and consistent containers that can run anywhere."*

---

# 📖 Introduction

Modern software applications often need to run consistently across multiple environments, including:

- Developer laptops
- Testing environments
- Staging servers
- Production servers
- Cloud platforms

Traditionally, developers faced the common problem:

> **"It works on my machine."**

Applications that worked perfectly during development frequently failed in production because of differences in:

- Operating Systems
- Library Versions
- Runtime Environments
- Dependencies
- Configuration

Docker solves this problem by packaging an application together with everything it needs into a **Container**.

Containers ensure that applications behave exactly the same regardless of where they are executed.

Docker has become one of the most important technologies in modern DevOps and cloud-native development. It serves as the foundation upon which Kubernetes orchestrates applications.

Throughout this handbook, Docker is used to package every application before deploying it to Amazon EKS.

---

# 🎯 Learning Objectives

After completing this chapter, you will understand:

- What Docker is
- Why Docker is used
- Virtual Machines vs Containers
- Docker Architecture
- Docker Engine
- Docker Images
- Docker Containers
- Docker Hub
- Dockerfile
- Docker Volumes
- Docker Networks
- Docker Compose
- Docker Registry
- Docker Best Practices
- Docker in this project

---

# 🤔 Why Docker?

Imagine developing a Node.js application.

Your laptop has:

- Node.js 22
- Ubuntu
- npm 10

Production server has:

- Node.js 18
- Different Linux version
- Different libraries

Application fails.

Docker packages:

- Application
- Runtime
- Dependencies
- Environment

Everything becomes portable.

---

# 🐳 What is Docker?

Docker is a containerization platform that packages applications into isolated environments called **Containers**.

A Docker container contains:

- Application Code
- Runtime
- Libraries
- Dependencies
- Configuration

Everything required to run the application.

---

# 📦 What is a Container?

A Container is a lightweight isolated environment.

Unlike a Virtual Machine, a container shares the host operating system kernel.

Example:

```text
Container

├── Application
├── Libraries
├── Runtime
└── Dependencies
```

Containers start within seconds and consume far fewer resources than virtual machines.

---

# 💻 Virtual Machines vs Containers

## Traditional Virtual Machine

```text
Application

↓

Guest OS

↓

Hypervisor

↓

Host OS

↓

Hardware
```

Every Virtual Machine includes its own operating system.

Problems:

- Heavy
- Slow Startup
- High Memory Usage

---

## Docker Containers

```text
Application

↓

Container

↓

Docker Engine

↓

Host Operating System

↓

Hardware
```

Containers share the host operating system.

Benefits:

- Lightweight
- Fast
- Efficient
- Portable

---

# 🏗 Docker Architecture

Docker consists of several components.

```text
Developer

      │

Docker CLI

      │

Docker Engine

      │

Docker Images

      │

Docker Containers
```

---

# ⚙ Docker Engine

Docker Engine is responsible for:

- Building Images
- Running Containers
- Managing Networks
- Managing Volumes

It acts as the core runtime of Docker.

---

# 🖼 Docker Images

A Docker Image is a **read-only template** used to create containers.

Think of an image as a blueprint.

Example:

```text
Docker Image

↓

Container 1

↓

Container 2

↓

Container 3
```

One image can create many containers.

---

# 📦 Docker Containers

A Docker Container is a running instance of an image.

Example:

```
nginx Image

↓

Running Container
```

Stopping the container does not remove the image.

---

# 📄 Dockerfile

A Dockerfile contains instructions for building an image.

Example:

```dockerfile
FROM node:22-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm","start"]
```

Docker reads the Dockerfile and builds an image automatically.

---

# 🔄 Docker Image Build Process

```text
Application Source Code

      │

Dockerfile

      │

docker build

      │

Docker Image

      │

docker run

      │

Container
```

---

# 📚 Docker Layers

Docker Images are built using layers.

Example:

```dockerfile
FROM node:22

COPY package.json

RUN npm install

COPY .
```

Each instruction creates a new layer.

Advantages:

- Faster Builds
- Layer Caching
- Reduced Storage
- Efficient Downloads

---

# 📂 Docker Registry

Docker Images are stored inside a Registry.

Popular registries:

| Registry | Purpose |
|----------|----------|
| Docker Hub | Public Images |
| GitHub Container Registry (GHCR) | GitHub Images |
| Amazon ECR | AWS Images |
| Google Artifact Registry | GCP Images |

---

# 🌍 Docker Hub

Docker Hub is Docker's official public registry.

Popular images:

- nginx
- redis
- postgres
- mysql
- node
- python

Example:

```bash
docker pull nginx
```

---

# 🚀 Running Containers

Run Nginx:

```bash
docker run nginx
```

Run with a custom name:

```bash
docker run --name web nginx
```

Run in detached mode:

```bash
docker run -d nginx
```

---

# 📋 Common Docker Commands

## Check Docker Version

```bash
docker --version
```

---

## List Images

```bash
docker images
```

---

## List Running Containers

```bash
docker ps
```

---

## List All Containers

```bash
docker ps -a
```

---

## Build an Image

```bash
docker build -t backend:v1 .
```

---

## Run a Container

```bash
docker run -d -p 3000:3000 backend:v1
```

---

## Stop a Container

```bash
docker stop <container-id>
```

---

## Remove a Container

```bash
docker rm <container-id>
```

---

## Remove an Image

```bash
docker rmi backend:v1
```

---

# 🌐 Docker Networking

Containers communicate using Docker Networks.

Network Types:

- Bridge
- Host
- None
- Overlay
- Macvlan

Example:

```text
Container A

       │

Bridge Network

       │

Container B
```

---

# 💾 Docker Volumes

Containers are temporary.

If a container is deleted, its data disappears.

Volumes provide persistent storage.

Example:

```bash
docker volume create postgres-data
```

Benefits:

- Persistent Data
- Database Storage
- Shared Data
- Easy Backup

---

# 🐙 Docker Compose

Docker Compose manages multiple containers.

Example:

```yaml
services:

  backend:
    build: .

  postgres:
    image: postgres:17

  redis:
    image: redis:7
```

Start everything:

```bash
docker compose up -d
```

Docker Compose is excellent for local development but is generally replaced by Kubernetes in production.

---

# 🔒 Docker Security Best Practices

- Use official base images
- Keep images small
- Scan images for vulnerabilities
- Avoid running containers as root
- Pin image versions
- Use `.dockerignore`
- Remove unnecessary packages
- Keep secrets outside images
- Regularly update base images

---

# 📁 .dockerignore

The `.dockerignore` file prevents unnecessary files from being included in the image.

Example:

```text
node_modules
.git
.env
README.md
coverage
```

Benefits:

- Smaller images
- Faster builds
- Improved security

---

# 🚀 Docker in This Project

Our workflow uses Docker to package every application.

```text
Developer

      │

Source Code

      │

Dockerfile

      │

docker build

      │

Docker Image

      │

GitHub Actions

      │

GitHub Container Registry (GHCR)

      │

ArgoCD

      │

Amazon EKS

      │

Pods
```

Docker images are stored in **GitHub Container Registry (GHCR)**.

---

# 🏗 Multi-Stage Builds

Multi-stage builds create smaller production images.

Example:

```dockerfile
# Build Stage
FROM node:22 AS builder

WORKDIR /app
COPY . .
RUN npm install
RUN npm run build

# Production Stage
FROM nginx:alpine

COPY --from=builder /app/dist /usr/share/nginx/html
```

Benefits:

- Smaller image size
- Improved security
- Faster deployments

---

# 🌟 Advantages of Docker

- Consistent environments
- Lightweight containers
- Fast startup
- Easy deployment
- Portability
- Resource efficiency
- Simplified CI/CD integration
- Cloud-native ready

---

# ⚠ Common Mistakes

❌ Using the `latest` image tag

❌ Running containers as root

❌ Copying `.env` files into images

❌ Creating unnecessarily large images

❌ Ignoring `.dockerignore`

❌ Installing development dependencies in production images

❌ Hardcoding secrets

❌ Not scanning images for vulnerabilities

---

# 💼 Real-World Example

Suppose a developer builds a Node.js backend.

1. The application source code is committed to GitHub.
2. GitHub Actions automatically builds a Docker image.
3. The image is tagged with the Git commit SHA and application version.
4. The image is pushed to GitHub Container Registry (GHCR).
5. ArgoCD detects the new version.
6. Kubernetes pulls the updated image.
7. A rolling update replaces the old Pods without downtime.

Docker ensures the exact same application runs in development, testing, and production.

---

# 📝 Summary

Docker revolutionized software deployment by introducing lightweight, portable containers that package applications with all required dependencies. It eliminates environment inconsistencies and provides a consistent runtime across development, testing, and production.

In this project, Docker acts as the bridge between application development and Kubernetes deployment. Every application is built into a Docker image, stored in GitHub Container Registry, and deployed automatically to Amazon EKS through a GitOps workflow.

---

# 🎯 Key Takeaways

- Docker packages applications into portable containers.
- Images are templates used to create containers.
- Dockerfiles define how images are built.
- Containers are lightweight compared to virtual machines.
- Docker Compose simplifies multi-container local development.
- Volumes provide persistent storage.
- Registries such as GHCR store Docker images.
- Multi-stage builds reduce image size and improve security.
- Docker is the foundation for Kubernetes deployments.

---

## 📚 Next Chapter

➡️ **11-GitHub-Container-Registry-GHCR.md**