# Helm

> *"Helm is the package manager for Kubernetes. It simplifies the deployment, configuration, versioning, and lifecycle management of Kubernetes applications using reusable packages called Charts."*

---

# 📖 Introduction

As Kubernetes clusters grow, managing applications becomes increasingly complex. Even a simple web application may require multiple Kubernetes resources such as:

- Deployment
- Service
- ConfigMap
- Secret
- Ingress or Gateway
- Persistent Volume Claim
- Service Account
- Role
- RoleBinding

Managing dozens or even hundreds of YAML files manually is difficult, especially across multiple environments such as development, staging, and production.

Helm solves this problem by allowing developers and DevOps engineers to package Kubernetes resources into reusable **Helm Charts**. Instead of applying many YAML files individually, an entire application can be installed with a single command.

Throughout this project, Helm is used to install and manage infrastructure components such as:

- ArgoCD
- Prometheus
- Grafana
- Elasticsearch
- Kibana
- Gateway API
- AWS Load Balancer Controller
- Metrics Server
- ArgoCD Image Updater

---

# 🎯 Learning Objectives

After completing this chapter, you will understand:

- What Helm is
- Why Helm is used
- Helm Architecture
- Helm Charts
- Releases
- Repositories
- Values Files
- Templates
- Helm Commands
- Chart Structure
- Helm Lifecycle
- Helm Best Practices
- Helm in this project

---

# 🤔 Why Helm?

Imagine deploying Prometheus manually.

You might need:

- Deployment
- Service
- ConfigMap
- Secret
- ServiceAccount
- ClusterRole
- ClusterRoleBinding
- PersistentVolumeClaim
- StatefulSet

This could involve **30–100 YAML files**.

With Helm:

```bash
helm install prometheus prometheus-community/kube-prometheus-stack
```

One command installs the complete monitoring stack.

---

# 📦 What is Helm?

Helm is the official package manager for Kubernetes.

Think of it like:

| Platform | Package Manager |
|-----------|-----------------|
| Ubuntu | apt |
| macOS | Homebrew |
| Node.js | npm |
| Python | pip |
| Kubernetes | Helm |

Helm allows you to install, upgrade, rollback, and uninstall Kubernetes applications easily.

---

# 🏗 Helm Architecture

```text
Developer

      │

helm install

      │

Helm CLI

      │

Helm Chart

      │

Templates

      │

Generated YAML

      │

Kubernetes API

      │

Cluster
```

Helm generates standard Kubernetes manifests from templates and sends them to the Kubernetes API.

---

# 📦 What is a Helm Chart?

A **Chart** is a collection of files that define a Kubernetes application.

A chart contains:

- Kubernetes templates
- Default configuration
- Metadata
- Dependencies

Think of a Chart as an application package.

---

# 📂 Helm Chart Structure

A typical Helm Chart looks like:

```text
my-app/

├── Chart.yaml
├── values.yaml
├── charts/
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   ├── configmap.yaml
│   ├── secret.yaml
│   └── serviceaccount.yaml
└── README.md
```

---

# 📄 Chart.yaml

This file contains chart metadata.

Example:

```yaml
apiVersion: v2
name: my-app
description: Sample Helm Chart
version: 1.0.0
appVersion: "1.0.0"
```

---

# ⚙ values.yaml

The `values.yaml` file stores configurable values.

Example:

```yaml
replicaCount: 3

image:
  repository: ghcr.io/company/backend
  tag: "1.0.0"

service:
  port: 80
```

Instead of modifying templates directly, you simply change values.

---

# 📝 Templates

Templates generate Kubernetes YAML files.

Example:

```yaml
spec:
  replicas: {{ .Values.replicaCount }}
```

If:

```yaml
replicaCount: 5
```

Helm generates:

```yaml
replicas: 5
```

This makes charts reusable across environments.

---

# 🚀 Helm Releases

Whenever a chart is installed, Helm creates a **Release**.

Example:

```bash
helm install monitoring prometheus-community/kube-prometheus-stack
```

Here:

- Release Name → `monitoring`
- Chart → `kube-prometheus-stack`

Multiple releases of the same chart can exist in one cluster.

---

# 📚 Helm Repositories

Charts are stored in repositories.

Popular repositories:

| Repository | Purpose |
|------------|----------|
| Bitnami | General Applications |
| Prometheus Community | Monitoring |
| Grafana | Dashboards |
| Elastic | Logging |
| Argo | GitOps |
| Jetstack | cert-manager |

Add a repository:

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

Update repositories:

```bash
helm repo update
```

---

# 🔄 Helm Lifecycle

```text
Create Chart

      │

Package

      │

Repository

      │

helm install

      │

Release

      │

helm upgrade

      │

helm rollback

      │

helm uninstall
```

---

# ⚡ Common Helm Commands

## Add Repository

```bash
helm repo add argo https://argoproj.github.io/argo-helm
```

---

## Update Repository

```bash
helm repo update
```

---

## Search Charts

```bash
helm search repo prometheus
```

---

## Install

```bash
helm install grafana grafana/grafana
```

---

## Upgrade

```bash
helm upgrade grafana grafana/grafana
```

---

## Rollback

```bash
helm rollback grafana 1
```

---

## Uninstall

```bash
helm uninstall grafana
```

---

## List Releases

```bash
helm list
```

---

## Show Values

```bash
helm show values grafana/grafana
```

---

# 🌍 Environment-Specific Configuration

Different environments often require different settings.

Example:

```text
values-dev.yaml

values-stage.yaml

values-prod.yaml
```

Deploy production:

```bash
helm install backend ./backend -f values-prod.yaml
```

Benefits:

- Environment isolation
- Easy configuration
- Reusability

---

# 📦 Helm Dependencies

Charts can depend on other charts.

Example:

```
Application

↓

PostgreSQL

↓

Redis

↓

cert-manager
```

Dependencies are defined in `Chart.yaml`.

---

# ☸ Helm in This Project

Helm installs nearly all platform components.

```text
Helm

├── ArgoCD
├── Prometheus
├── Grafana
├── AlertManager
├── Elasticsearch
├── Kibana
├── AWS Load Balancer Controller
├── Metrics Server
├── Gateway API
└── ArgoCD Image Updater
```

This significantly reduces deployment complexity.

---

# 🔄 Complete Deployment Workflow

```text
Terraform

      │

Amazon EKS

      │

Helm

      │

Install Platform Components

      │

ArgoCD

      │

Git Repository

      │

Applications

      │

Pods
```

---

# 🎨 Creating Your Own Chart

Generate a new chart:

```bash
helm create backend
```

This creates:

```text
backend/

├── Chart.yaml
├── values.yaml
├── templates/
└── charts/
```

You can then customize it for your application.

---

# 📈 Helm Versioning

Helm tracks application versions.

Example:

| Chart Version | App Version |
|---------------|-------------|
| 1.0.0 | 1.0.0 |
| 1.1.0 | 1.1.2 |
| 2.0.0 | 2.0.0 |

Chart version and application version are managed independently.

---

# 🌟 Advantages of Helm

- Faster deployments
- Reusable templates
- Version management
- Easy upgrades
- Rollback support
- Environment-specific configuration
- Simplified dependency management
- Community-maintained charts
- Reduced YAML duplication

---

# ⚠ Common Mistakes

❌ Editing generated Kubernetes resources manually

❌ Hardcoding configuration values

❌ Ignoring `values.yaml`

❌ Not versioning charts

❌ Storing secrets directly in charts

❌ Using one values file for every environment

❌ Forgetting to update chart dependencies

---

# 🏢 Real-World Example

Suppose you need to deploy a monitoring platform.

Without Helm, you would manually apply dozens of YAML manifests for Prometheus, Grafana, Alertmanager, exporters, ConfigMaps, Services, and RBAC resources.

With Helm:

1. Add the Prometheus Community repository.
2. Update repository indexes.
3. Install the `kube-prometheus-stack` chart.
4. Customize monitoring through `values.yaml`.
5. Upgrade the installation whenever new versions are released.
6. Roll back instantly if an upgrade introduces problems.

Helm simplifies the entire lifecycle while maintaining consistency across environments.

---

# 📝 Summary

Helm is an essential tool for Kubernetes administrators and DevOps engineers. By packaging Kubernetes resources into reusable charts, Helm simplifies application deployment, configuration, upgrades, and rollbacks.

In this project, Helm is responsible for installing and managing nearly all platform services, enabling rapid, repeatable, and production-ready deployments.

Combined with Terraform for infrastructure provisioning and ArgoCD for GitOps, Helm forms a key part of the modern Kubernetes deployment workflow.

---

# 🎯 Key Takeaways

- Helm is the package manager for Kubernetes.
- Charts package Kubernetes resources into reusable templates.
- `values.yaml` provides configurable application settings.
- Releases track installed chart instances.
- Helm supports upgrades, rollbacks, and dependency management.
- Community repositories provide production-ready charts.
- Helm greatly reduces YAML duplication and operational complexity.

---

## 📚 Next Chapter

➡️ **10-GitOps-and-ArgoCD.md**