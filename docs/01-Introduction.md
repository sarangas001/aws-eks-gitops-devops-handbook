# Introduction

<div align="center">

# 🚀 Production-Ready DevOps on AWS using Terraform, Kubernetes (EKS), GitOps & ArgoCD

### A Complete Hands-on Learning Guide for Building, Deploying, Monitoring, and Managing Cloud-Native Applications

![AWS](https://img.shields.io/badge/AWS-EKS-orange?style=for-the-badge&logo=amazonaws)
![Terraform](https://img.shields.io/badge/Terraform-IaC-purple?style=for-the-badge&logo=terraform)
![Kubernetes](https://img.shields.io/badge/Kubernetes-EKS-blue?style=for-the-badge&logo=kubernetes)
![GitHub Actions](https://img.shields.io/badge/GitHub-Actions-black?style=for-the-badge&logo=githubactions)
![ArgoCD](https://img.shields.io/badge/ArgoCD-GitOps-red?style=for-the-badge&logo=argo)
![Docker](https://img.shields.io/badge/Docker-Containers-blue?style=for-the-badge&logo=docker)

</div>

---

# 📖 About This Handbook

Welcome!

This handbook documents the journey of building a **production-ready cloud-native DevOps platform** on **Amazon Web Services (AWS)** using modern DevOps practices and GitOps principles.

Rather than learning each tool separately, this guide demonstrates how industry-standard DevOps technologies work together to automate infrastructure provisioning, application deployment, monitoring, logging, scaling, and continuous delivery.

The project is based on deploying a **microservices-based e-commerce application** to a **private Amazon Elastic Kubernetes Service (EKS)** cluster using **Terraform**, **GitHub Actions**, and **ArgoCD** while implementing GitOps workflows.

The goal of this handbook is not simply to teach commands but to explain **why each component exists**, **how they communicate**, and **how production DevOps systems are designed in real organizations.**

---

# 🎯 Objectives

By the end of this handbook, you will understand how to:

- Provision AWS infrastructure using Terraform
- Build a secure Virtual Private Cloud (VPC)
- Configure public and private subnets
- Deploy a private Amazon EKS cluster
- Configure Kubernetes networking
- Deploy applications using GitOps
- Build CI/CD pipelines using GitHub Actions
- Store container images in GitHub Container Registry (GHCR)
- Automatically deploy new images using ArgoCD Image Updater
- Configure Gateway API for traffic routing
- Use Route53 and ACM for DNS and HTTPS
- Monitor Kubernetes using Prometheus and Grafana
- Collect logs using Filebeat, Elasticsearch, and Kibana
- Configure Slack alerts using AlertManager
- Scale workloads using Horizontal Pod Autoscaler (HPA)
- Apply production-ready DevOps best practices

---

# 🌍 Why This Project?

Learning DevOps by studying individual tools often leaves a gap between theory and real-world implementation.

For example:

- Learning Docker alone doesn't explain how containers are deployed.
- Learning Kubernetes alone doesn't explain continuous deployment.
- Learning Terraform alone doesn't explain how infrastructure supports applications.
- Learning GitHub Actions alone doesn't explain GitOps.

This project bridges those gaps by showing how every tool integrates into a complete DevOps ecosystem.

Instead of isolated tutorials, you'll build a production-style platform where every component has a clear purpose.

---

# 🏗 Project Overview

The project deploys a cloud-native e-commerce application composed of multiple microservices.

The application is hosted on a private Kubernetes cluster running on Amazon EKS.

Infrastructure is created using Terraform.

Application delivery follows GitOps principles using ArgoCD.

GitHub Actions automate image builds and security scanning.

Monitoring and logging provide complete observability.

Everything is automated from infrastructure creation to application deployment.

---

# ☁ High-Level Architecture

```text
                   Developer

                       │
                 Git Push Code
                       │
                       ▼
                GitHub Repository
                       │
             GitHub Actions (CI)
                       │
        Build • Scan • Push Images
                       │
                       ▼
      GitHub Container Registry (GHCR)
                       │
                       ▼
        ArgoCD Image Updater
                       │
                       ▼
              Update Git Manifest
                       │
                       ▼
                 ArgoCD Sync
                       │
                       ▼
               Amazon EKS Cluster
                       │
         Kubernetes Deployments
                       │
            Services → Pods
                       │
                    Users
```

---

# 🧰 Technology Stack

| Category | Technology |
|----------|------------|
| Cloud Provider | Amazon Web Services (AWS) |
| Infrastructure as Code | Terraform |
| Container Platform | Docker |
| Container Orchestration | Kubernetes |
| Managed Kubernetes | Amazon EKS |
| CI | GitHub Actions |
| CD | ArgoCD |
| GitOps | ArgoCD + Git |
| Container Registry | GitHub Container Registry (GHCR) |
| Package Manager | Helm |
| Networking | Gateway API |
| Load Balancer | AWS Load Balancer Controller |
| DNS | Route53 |
| SSL Certificates | AWS Certificate Manager |
| Monitoring | Prometheus |
| Dashboards | Grafana |
| Alerting | AlertManager |
| Logging | Filebeat |
| Log Storage | Elasticsearch |
| Log Visualization | Kibana |
| Notifications | Slack |

---

# 📚 What You Will Learn

This handbook covers much more than deployment.

It explains the architecture, design decisions, and production concepts behind each technology.

Topics include:

- DevOps Fundamentals
- Cloud Computing
- Infrastructure as Code
- Terraform
- AWS Networking
- Kubernetes
- Docker
- Amazon EKS
- Helm
- GitOps
- GitHub Actions
- ArgoCD
- Gateway API
- AWS Load Balancer Controller
- ExternalDNS
- Route53
- AWS Certificate Manager
- Prometheus
- Grafana
- AlertManager
- Elasticsearch
- Kibana
- Horizontal Pod Autoscaler
- IRSA
- Pod Identity
- Security Best Practices
- Production Monitoring
- Troubleshooting
- CI/CD Pipelines

---

# 🎓 Who Is This Handbook For?

This guide is designed for:

- Students learning DevOps
- Software Engineers
- Cloud Engineers
- DevOps Engineers
- Site Reliability Engineers (SRE)
- Platform Engineers
- Kubernetes Beginners
- AWS Learners
- Professionals preparing for DevOps interviews

No advanced Kubernetes knowledge is required.

Each topic builds upon the previous one in a structured learning path.

---

# 📈 Learning Roadmap

```text
Linux Fundamentals
        │
        ▼
Git & GitHub
        │
        ▼
Docker
        │
        ▼
Terraform
        │
        ▼
AWS Networking
        │
        ▼
Amazon EKS
        │
        ▼
Kubernetes
        │
        ▼
Helm
        │
        ▼
GitHub Actions
        │
        ▼
GitOps
        │
        ▼
ArgoCD
        │
        ▼
Monitoring
        │
        ▼
Logging
        │
        ▼
Production Deployment
```

---

# 💡 Learning Philosophy

This handbook focuses on understanding concepts rather than memorizing commands.

For every technology, we will answer four important questions:

1. What is it?
2. Why do we need it?
3. How does it work?
4. How is it implemented in this project?

This approach helps build long-term knowledge that can be applied to real-world DevOps projects.

---

# 📂 Structure of This Handbook

The documentation is organized into chapters, each focusing on a specific technology or concept.

Each chapter includes:

- Introduction
- Theory
- Architecture
- Practical Implementation
- Best Practices
- Common Mistakes
- Interview Questions
- Summary

This structure makes it easy to revisit topics whenever needed.

---

# 🚀 What You'll Build

By the end of this guide, you will have built a complete production-ready DevOps platform featuring:

- Automated AWS Infrastructure Provisioning
- Private Amazon EKS Cluster
- Secure Networking
- GitOps-Based Continuous Delivery
- Automated CI/CD Pipelines
- Container Image Management
- HTTPS Traffic Routing
- Centralized Monitoring
- Centralized Logging
- Slack Alerting
- Automatic DNS Management
- Auto Scaling
- Production-Ready Kubernetes Workloads

---

# 🏁 Final Thoughts

Modern DevOps is much more than writing deployment scripts.

It is about creating reliable, secure, automated, and observable systems that allow teams to deliver software quickly and confidently.

This handbook is intended to be your long-term reference for understanding how modern cloud-native platforms are designed and operated. As you progress through each chapter, you'll gain both theoretical knowledge and practical experience with the tools and workflows used by professional DevOps teams.

Let's begin the journey of building a complete production-ready DevOps platform—one chapter at a time.

---

**Next Chapter ➜ `02-What-is-DevOps.md`**