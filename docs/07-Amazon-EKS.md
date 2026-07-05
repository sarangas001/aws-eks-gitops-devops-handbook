# Amazon Elastic Kubernetes Service (Amazon EKS)

> *"Amazon Elastic Kubernetes Service (Amazon EKS) is a fully managed Kubernetes service that simplifies deploying, managing, and scaling containerized applications on AWS without managing the Kubernetes control plane."*

---

# 📖 Introduction

As organizations adopt microservices and containerized applications, managing containers at scale becomes increasingly challenging. Running a few Docker containers on a single server is straightforward, but managing hundreds or thousands of containers across multiple servers requires an orchestration platform.

**Kubernetes** has become the industry standard for container orchestration. However, operating a Kubernetes cluster yourself involves installing and maintaining the control plane, upgrading Kubernetes versions, ensuring high availability, securing the cluster, and monitoring its health.

Amazon **Elastic Kubernetes Service (EKS)** eliminates much of this operational burden by providing a **fully managed Kubernetes control plane**. AWS manages the Kubernetes API server, etcd database, scheduler, and controller manager while allowing users to focus on deploying and managing applications.

In this project, Amazon EKS serves as the central platform where all microservices are deployed using GitOps with ArgoCD.

---

# 🎯 Learning Objectives

After completing this chapter, you will understand:

- What Amazon EKS is
- Kubernetes vs Amazon EKS
- Why Amazon EKS is used
- EKS Architecture
- Control Plane
- Worker Nodes
- Node Groups
- Managed Node Groups
- Private Clusters
- EKS Networking
- IAM Integration
- Cluster Security
- EKS Best Practices
- Amazon EKS in this project

---

# ☸ What is Kubernetes?

Kubernetes (K8s) is an open-source container orchestration platform originally developed by Google.

It automates:

- Container deployment
- Scaling
- Service discovery
- Load balancing
- Self-healing
- Rolling updates
- Resource scheduling

Instead of manually managing containers, Kubernetes automatically ensures applications remain available and healthy.

---

# 🚀 What is Amazon EKS?

Amazon Elastic Kubernetes Service (Amazon EKS) is AWS's managed Kubernetes platform.

AWS manages:

- Kubernetes Control Plane
- API Server
- etcd Database
- Scheduler
- Controller Manager
- Cluster Availability
- Security Updates

You manage:

- Worker Nodes
- Applications
- Pods
- Services
- Deployments
- Networking Policies

This shared responsibility allows teams to focus on application delivery instead of cluster maintenance.

---

# 🤔 Why Amazon EKS?

Managing Kubernetes yourself is complex.

Without EKS, you must maintain:

- Control Plane
- etcd
- Certificates
- Upgrades
- High Availability
- Backups
- Security

Amazon EKS automates these tasks.

Benefits include:

- Fully Managed Control Plane
- High Availability
- Automatic Updates
- Native AWS Integration
- Enterprise Security
- Scalability
- Reliability

---

# 🏗 Amazon EKS Architecture

```text
                 Amazon EKS

        ┌──────────────────────────┐
        │   Kubernetes Control Plane │
        │                          │
        │ API Server               │
        │ Scheduler                │
        │ Controller Manager       │
        │ etcd                     │
        └──────────────────────────┘
                    │
        ───────────────────────────────
                    │
      Managed Node Groups (EC2)
                    │
        ┌───────────┴───────────┐
        │                       │
      Worker Node           Worker Node
        │                       │
     Kubernetes Pods      Kubernetes Pods
```

---

# 🧠 Kubernetes Control Plane

The **Control Plane** is the brain of the Kubernetes cluster.

Components include:

### API Server

Receives all Kubernetes requests.

Example:

```bash
kubectl get pods
```

Every `kubectl` command communicates with the API Server.

---

### etcd

A distributed key-value database that stores:

- Cluster configuration
- Secrets
- Deployments
- Services
- Nodes
- Pods

Think of **etcd** as the cluster's database.

---

### Scheduler

Determines where Pods should run.

It evaluates:

- CPU
- Memory
- Node availability
- Affinity rules

---

### Controller Manager

Continuously compares the desired cluster state with the current state.

If something fails, it automatically restores the desired state.

Example:

Desired:

```
3 Pods
```

Current:

```
2 Pods
```

Controller Manager creates one additional Pod automatically.

---

# 🖥 Worker Nodes

Worker Nodes are EC2 instances where applications actually run.

Each worker node contains:

- kubelet
- kube-proxy
- Container Runtime
- Pods

Example:

```text
Worker Node

│

├── kubelet

├── kube-proxy

├── Container Runtime

└── Pods
```

---

# 📦 Managed Node Groups

Amazon EKS provides **Managed Node Groups**.

AWS automatically manages:

- Node provisioning
- Node replacement
- Updates
- Scaling
- Health monitoring

Benefits:

- Simplified management
- Automatic upgrades
- Better reliability

---

# 🔒 Private EKS Cluster

In this project, the Kubernetes cluster is **private**.

Architecture:

```text
Internet

      │

Bastion Host

      │

Private VPC

      │

Amazon EKS

      │

Worker Nodes
```

Advantages:

- Better Security
- No Public Worker Nodes
- Reduced Attack Surface
- Compliance

---

# 🌐 EKS Networking

Our networking architecture consists of:

```text
VPC

├── Public Subnets
│      │
│      ├── Bastion Host
│      └── Application Load Balancer
│
└── Private Subnets
       │
       ├── Worker Node 1
       ├── Worker Node 2
       └── Kubernetes Pods
```

Applications remain private while being securely exposed through an AWS Load Balancer.

---

# 🔐 IAM Integration

Amazon EKS integrates with AWS IAM.

This allows secure authentication between:

- Kubernetes
- AWS Services
- Pods
- Users

Examples:

- IAM Roles for Service Accounts (IRSA)
- EKS Pod Identity
- IAM Authentication

These features eliminate the need to store AWS credentials inside containers.

---

# 🚀 Deploying Applications

Applications are deployed as Kubernetes resources.

Typical deployment flow:

```text
GitHub

      │

GitHub Actions

      │

GHCR

      │

ArgoCD

      │

Amazon EKS

      │

Deployment

      │

Pods
```

Every deployment is automated using GitOps.

---

# 🔄 High Availability

Amazon EKS automatically distributes worker nodes across multiple Availability Zones.

Example:

```text
ap-south-1

├── AZ-A

│      Worker Node

├── AZ-B

│      Worker Node

└── AZ-C

       Worker Node
```

Benefits:

- Fault Tolerance
- High Availability
- Disaster Recovery

---

# 📊 Scaling

EKS supports multiple scaling methods.

## Horizontal Pod Autoscaler (HPA)

Automatically increases or decreases Pods.

Example:

```
2 Pods

↓

Traffic Increases

↓

6 Pods
```

---

## Cluster Autoscaler

Automatically adds new EC2 worker nodes when required.

---

# 🛡 Security Features

Amazon EKS provides:

- IAM Authentication
- Private Networking
- Security Groups
- Network Policies
- Kubernetes RBAC
- Secrets Management
- Encryption
- Pod Identity
- IRSA

---

# 📂 Amazon EKS Components

| Component | Purpose |
|-----------|----------|
| Control Plane | Cluster Management |
| Worker Nodes | Run Containers |
| Pods | Application Instances |
| Services | Networking |
| Deployments | Manage Pods |
| ConfigMaps | Configuration |
| Secrets | Sensitive Data |
| Namespaces | Resource Isolation |

---

# 🏗 Amazon EKS in This Project

Terraform provisions:

- Amazon EKS Cluster
- Managed Node Groups
- IAM Roles
- Security Groups
- Networking
- Cluster Endpoint

After the cluster is created:

- kubectl is configured
- Helm is installed
- Gateway API is deployed
- AWS Load Balancer Controller is installed
- ArgoCD is installed
- Monitoring and Logging stacks are deployed

---

# 🔄 Complete EKS Workflow

```text
Terraform

      │

Creates

      │

Amazon EKS

      │

Managed Node Groups

      │

kubectl Access

      │

Helm

      │

Install Components

      │

Gateway API

      │

ArgoCD

      │

Applications

      │

Monitoring

      │

Logging
```

---

# 🌟 Advantages of Amazon EKS

- Fully Managed Kubernetes
- Highly Available Control Plane
- Automatic Upgrades
- AWS Integration
- Secure Networking
- Auto Scaling
- Enterprise Security
- Native IAM Support
- GitOps Ready

---

# ⚠ Common Mistakes

❌ Deploying Worker Nodes in Public Subnets

❌ Giving Pods AWS Access Keys

❌ Running Everything in the Default Namespace

❌ Ignoring Resource Limits

❌ Not Enabling Monitoring

❌ Exposing the Kubernetes API Publicly Without Restrictions

❌ Not Backing Up Kubernetes Resources

---

# 💼 Real-World Example

In this project, Terraform first provisions the Amazon EKS cluster along with its managed node groups. Once the infrastructure is ready, the DevOps engineer connects to the Bastion Host and configures `kubectl`.

Helm is then used to install the Gateway API, AWS Load Balancer Controller, ArgoCD, Prometheus, Grafana, Elasticsearch, Kibana, and other supporting components.

When developers push code to GitHub, GitHub Actions builds Docker images and publishes them to GitHub Container Registry (GHCR). ArgoCD Image Updater detects new image versions and updates the Kubernetes deployments automatically, ensuring that the cluster always runs the latest approved version of the application.

---

# 📝 Summary

Amazon Elastic Kubernetes Service (EKS) provides a fully managed Kubernetes platform that enables organizations to deploy and operate containerized applications without managing the Kubernetes control plane.

By integrating seamlessly with AWS services such as IAM, VPC, Route 53, and Application Load Balancers, EKS offers a secure, scalable, and highly available environment for running modern cloud-native workloads.

In this project, EKS serves as the core platform where all microservices, monitoring tools, logging systems, and GitOps workflows come together to create a production-ready DevOps ecosystem.

---

# 🎯 Key Takeaways

- Amazon EKS is a fully managed Kubernetes service.
- AWS manages the Kubernetes control plane.
- Worker Nodes run your applications.
- Managed Node Groups simplify node management.
- Private clusters improve security.
- EKS integrates natively with IAM, VPC, and AWS networking.
- Auto Scaling ensures applications can handle varying workloads.
- Amazon EKS is the foundation of the GitOps platform built in this handbook.

---

## 📚 Next Chapter

➡️ **08-Kubernetes.md**