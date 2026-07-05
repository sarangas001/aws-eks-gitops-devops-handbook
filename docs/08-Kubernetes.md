# Kubernetes

> *"Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, networking, and management of containerized applications. It is the industry standard for running cloud-native applications at scale."*

---

# 📖 Introduction

As modern applications evolved from monolithic architectures to microservices, managing hundreds or thousands of Docker containers became increasingly complex.

Imagine running:

- 10 Microservices
- 500 Containers
- 20 Servers
- Millions of Users

Managing these containers manually would be nearly impossible.

Questions arise such as:

- Which server should a container run on?
- What happens if a server crashes?
- How are containers scaled?
- How do services communicate?
- How are updates deployed without downtime?

This is where **Kubernetes** comes in.

Originally developed by Google and later donated to the Cloud Native Computing Foundation (CNCF), Kubernetes has become the world's most popular container orchestration platform.

Throughout this handbook, Kubernetes serves as the runtime environment where all applications, monitoring systems, logging stacks, and GitOps tools are deployed.

---

# 🎯 Learning Objectives

By the end of this chapter, you will understand:

- What Kubernetes is
- Why Kubernetes is needed
- Kubernetes Architecture
- Control Plane
- Worker Nodes
- Pods
- ReplicaSets
- Deployments
- Services
- Namespaces
- Labels & Selectors
- ConfigMaps
- Secrets
- Volumes
- Persistent Storage
- Scheduling
- Self-Healing
- Scaling
- Kubernetes Best Practices

---

# 🤔 Why Kubernetes?

Running Docker containers manually works well for development.

Example:

```bash
docker run nginx
```

However, production environments require:

- High Availability
- Auto Scaling
- Rolling Updates
- Load Balancing
- Service Discovery
- Fault Tolerance
- Monitoring
- Security

Docker alone cannot efficiently manage these requirements.

Kubernetes automates them.

---

# 🚀 What is Kubernetes?

Kubernetes is a container orchestration platform that manages containerized applications across multiple machines.

It automates:

- Deployment
- Scaling
- Networking
- Scheduling
- Recovery
- Load Balancing
- Configuration Management

Kubernetes constantly ensures that the **actual state** of the cluster matches the **desired state** defined by the user.

---

# 🏗 Kubernetes Architecture

```text
                 Kubernetes Cluster

        ┌────────────────────────────┐
        │      Control Plane         │
        ├────────────────────────────┤
        │ API Server                 │
        │ Scheduler                  │
        │ Controller Manager         │
        │ etcd                       │
        └────────────────────────────┘
                     │
     ───────────────────────────────────
                     │
      ┌──────────────┴──────────────┐
      │                             │
 Worker Node                  Worker Node
      │                             │
 ┌───────────────┐           ┌───────────────┐
 │ Pod           │           │ Pod           │
 │ Pod           │           │ Pod           │
 │ Pod           │           │ Pod           │
 └───────────────┘           └───────────────┘
```

---

# 🧠 Kubernetes Control Plane

The Control Plane manages the entire Kubernetes cluster.

It is responsible for:

- Scheduling Pods
- Managing Deployments
- Maintaining Cluster State
- API Requests
- Node Management

Components:

- API Server
- Scheduler
- Controller Manager
- etcd

In Amazon EKS, AWS manages the Control Plane.

---

# 🌐 API Server

The API Server is the main entry point into Kubernetes.

Every request passes through it.

Example:

```bash
kubectl get pods
```

The API Server:

- Validates requests
- Authenticates users
- Updates cluster state
- Returns responses

---

# 🗄 etcd

etcd is Kubernetes' distributed key-value database.

It stores:

- Pods
- Nodes
- Deployments
- Secrets
- ConfigMaps
- Cluster configuration

Think of etcd as the **brain's memory**.

Without etcd, Kubernetes cannot recover cluster information.

---

# 📅 Scheduler

The Scheduler determines where Pods should run.

It considers:

- CPU availability
- Memory usage
- Node health
- Resource requests
- Node affinity
- Taints and tolerations

The Scheduler ensures efficient workload distribution.

---

# 🔄 Controller Manager

The Controller Manager continuously compares:

**Desired State**

vs

**Current State**

Example:

Desired:

```
5 Pods
```

Current:

```
3 Pods
```

Controller Manager creates two additional Pods automatically.

This process is known as **Self-Healing**.

---

# 🖥 Worker Nodes

Worker Nodes are machines where application containers actually run.

Each Worker Node contains:

```text
Worker Node

├── kubelet
├── kube-proxy
├── Container Runtime
└── Pods
```

---

# 🔧 kubelet

The kubelet is an agent running on every worker node.

Responsibilities:

- Starts Pods
- Monitors Pods
- Reports status
- Communicates with the API Server

---

# 🌐 kube-proxy

kube-proxy manages networking between Pods and Services.

Responsibilities:

- Traffic Routing
- Load Balancing
- Network Rules

---

# 📦 Container Runtime

The Container Runtime executes containers.

Examples:

- containerd
- CRI-O

(Docker was historically used but modern Kubernetes primarily uses containerd.)

---

# 📦 Pods

A **Pod** is the smallest deployable unit in Kubernetes.

A Pod contains:

- One or more containers
- Shared networking
- Shared storage

Example:

```text
Pod

├── Application Container
└── Sidecar Container
```

Most Pods contain a single container.

---

# 🔄 ReplicaSets

ReplicaSets ensure a specified number of Pods are always running.

Example:

Desired:

```
3 Pods
```

If one Pod crashes:

```
Running:

2 Pods
```

ReplicaSet automatically creates another Pod.

---

# 🚀 Deployments

Deployments manage ReplicaSets.

Features:

- Rolling Updates
- Rollbacks
- Version Management
- Scaling

Example:

```yaml
replicas: 3
```

Deployment ensures three Pods always exist.

---

# 🌐 Services

Pods have temporary IP addresses.

Services provide stable networking.

Types:

| Type | Purpose |
|------|----------|
| ClusterIP | Internal Communication |
| NodePort | External Testing |
| LoadBalancer | Cloud Load Balancer |
| ExternalName | External DNS |

---

# 🏷 Labels

Labels organize Kubernetes resources.

Example:

```yaml
app: backend

environment: production

team: platform
```

Labels simplify management.

---

# 🎯 Selectors

Selectors locate resources using labels.

Example:

```yaml
selector:

  app: backend
```

Services use selectors to discover Pods.

---

# 📂 Namespaces

Namespaces logically isolate resources.

Example:

```text
Cluster

├── default

├── monitoring

├── argocd

├── logging

└── production
```

Benefits:

- Isolation
- Organization
- Security

---

# ⚙ ConfigMaps

ConfigMaps store non-sensitive configuration.

Example:

```yaml
DATABASE_HOST=db.internal

PORT=8080
```

Applications read configuration without rebuilding Docker images.

---

# 🔐 Secrets

Secrets store sensitive information.

Examples:

- Passwords
- API Keys
- Tokens
- Certificates

Unlike ConfigMaps, Secrets are encoded and should be managed securely.

---

# 💾 Volumes

Containers are ephemeral.

If a container is deleted, its filesystem is lost.

Volumes provide persistent storage.

Examples:

- Database Storage
- Logs
- Uploaded Files

---

# 📦 Persistent Volumes (PV)

Persistent Volumes provide storage independent of Pods.

Benefits:

- Data survives Pod restarts
- Shared storage
- Cloud integration

On AWS, Persistent Volumes often use Amazon EBS.

---

# 📥 Persistent Volume Claims (PVC)

Applications request storage using PVCs.

Example:

```
Application

↓

PVC

↓

Persistent Volume

↓

Amazon EBS
```

---

# ⚖ Resource Requests and Limits

Kubernetes allows CPU and memory constraints.

Example:

```yaml
resources:

  requests:

    cpu: "500m"

    memory: "512Mi"

  limits:

    cpu: "1"

    memory: "1Gi"
```

Benefits:

- Prevents resource starvation
- Improves scheduling
- Protects cluster stability

---

# 📈 Scaling

## Manual Scaling

```bash
kubectl scale deployment backend --replicas=5
```

---

## Horizontal Pod Autoscaler (HPA)

Automatically adjusts replicas based on CPU or custom metrics.

```
Traffic Increases

↓

Pods Increase

↓

Traffic Decreases

↓

Pods Reduce
```

---

# 🔄 Rolling Updates

Kubernetes updates applications gradually.

```
Version 1

↓

Version 2

↓

No Downtime
```

Benefits:

- Zero downtime
- Easy rollback
- Safer deployments

---

# ♻ Self-Healing

If a Pod crashes:

```
Pod Crash

↓

ReplicaSet Detects Failure

↓

New Pod Created
```

Applications remain available without manual intervention.

---

# 🌐 Service Discovery

Pods communicate using Service names instead of IP addresses.

Example:

```
backend.default.svc.cluster.local
```

This ensures communication remains stable even if Pods are recreated.

---

# 🏗 Kubernetes Objects

| Object | Purpose |
|---------|----------|
| Pod | Smallest Deployable Unit |
| ReplicaSet | Maintain Pod Count |
| Deployment | Manage ReplicaSets |
| Service | Networking |
| Namespace | Isolation |
| ConfigMap | Configuration |
| Secret | Sensitive Data |
| PVC | Storage Request |
| PV | Persistent Storage |
| Ingress / Gateway | External Traffic |

---

# 🚀 Kubernetes Workflow

```text
Developer

      │

Git Push

      │

GitHub Actions

      │

Docker Image

      │

GitHub Container Registry

      │

ArgoCD

      │

Deployment

      │

ReplicaSet

      │

Pods

      │

Service

      │

Gateway API

      │

Application Load Balancer

      │

Users
```

---

# ☸ Kubernetes in This Project

The Kubernetes cluster hosts:

- Frontend Application
- Backend API
- Authentication Service
- PostgreSQL (if required)
- Redis
- ArgoCD
- ArgoCD Image Updater
- Prometheus
- Grafana
- Elasticsearch
- Kibana
- AlertManager
- Gateway API Components

Each application is deployed declaratively using YAML manifests managed through Git.

---

# 🌟 Advantages of Kubernetes

- Automatic Scaling
- Self-Healing
- High Availability
- Rolling Updates
- Easy Rollbacks
- Service Discovery
- Load Balancing
- Resource Optimization
- Cloud Portability
- GitOps Ready

---

# ⚠ Common Mistakes

❌ Running everything in the `default` namespace

❌ Not defining CPU or memory limits

❌ Hardcoding configuration inside containers

❌ Storing passwords in ConfigMaps

❌ Using the `latest` Docker image tag in production

❌ Ignoring readiness and liveness probes

❌ Running Pods without ReplicaSets or Deployments

❌ Not monitoring cluster health

---

# 💼 Real-World Example

Suppose your e-commerce application consists of:

- Frontend
- Product Service
- Order Service
- Payment Service
- Notification Service

Each service runs in its own Deployment with multiple Pod replicas.

When customer traffic increases during a sale, the Horizontal Pod Autoscaler automatically creates additional Pods. If one worker node fails, Kubernetes reschedules the affected Pods onto healthy nodes. Rolling updates ensure new application versions are deployed without interrupting users, while Services provide stable networking between all microservices.

---

# 📝 Summary

Kubernetes is the foundation of modern cloud-native application deployment. It automates container orchestration, ensuring applications remain scalable, resilient, and highly available.

By abstracting infrastructure complexity, Kubernetes allows development teams to focus on building software while the platform manages scheduling, networking, recovery, and scaling.

Throughout this DevOps project, Kubernetes serves as the platform where all applications, monitoring tools, and GitOps workflows are deployed and managed.

---

# 🎯 Key Takeaways

- Kubernetes automates the deployment and management of containers.
- Pods are the smallest deployable units.
- Deployments manage ReplicaSets and application updates.
- Services provide stable networking for Pods.
- ConfigMaps and Secrets separate configuration from application code.
- Persistent Volumes provide durable storage.
- Kubernetes automatically heals failed workloads.
- Scaling and rolling updates are built into the platform.
- Kubernetes is the core runtime environment for cloud-native applications.

---

## 📚 Next Chapter

➡️ **09-Docker.md**