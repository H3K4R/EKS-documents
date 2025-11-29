# EKS Course Learnings – Full Notes + Commands

This document combines your handwritten notes and the commands you practiced during the EKS course section. It includes explanations for eksctl, EKS managed node groups, EC2 pod limits, Helm, and Helm charts.

---

# 1. eksctl Overview

**eksctl** is a CLI tool for creating and managing Kubernetes clusters on **Amazon EKS**.

### Why use eksctl?

* Easier than console-based EKS creation.
* Abstracts a lot of complex setup (VPC, subnets, IAM roles, security groups, etc.).
* Works using CloudFormation behind the scenes.

---

# 2. EKS Cluster Creation Commands

### Basic Cluster Creation

```bash
eksctl create cluster --name eks-test --nodegroup-name ng-default --node-type t3.micro --nodes 2
```

✔ Creates an EKS cluster (eks-test)
✔ Adds node group with 2 EC2 t3.micro instances

---

### Cluster Creation with Specific Version

```bash
eksctl create cluster --name demoeks --version 1.14 --nodegroup-name ng-demoeks --node-type t3.micro --nodes 3 --managed
```

✔ Uses Kubernetes version **1.14**
✔ Creates **managed** node group
✔ Launches 3 × t3.micro worker nodes

---

### Create Cluster with Fargate Profile

```bash
eksctl create cluster --name mycluster --fargate
```

✔ Creates an EKS cluster using **serverless Fargate compute** instead of EC2 nodes.

---

### Upgrade Managed Nodegroup

```bash
eksctl upgrade nodegroup --name=managed-ng-1 --cluster=managed-cluster
```

✔ Upgrades AMI + security patches for EKS worker nodes.

---

# 3. EC2 Instance Types & Pod Limits

* Maximum number of pods per node depends on **EC2 instance type**.
* Larger instance = supports more pods.
* You should check the pod limits for your EC2 type.
  Example: "t3.micro pod limit" → search on Google

👉 Important for scheduling and avoiding **PodPending** issues.

---

# 4. EKS Managed Nodegroups

Managed node groups simplify node lifecycle management.

### How Managed Nodegroups Help

* EKS automatically manages and maintains EC2 worker nodes.
* AWS releases AMIs with fixes and security patches.
* You can bring a custom AMI if needed.
* Automated deployments of updated AMIs.
* No application downtime during upgrades.
* No user-managed orchestration overhead.
* Uses autoscaling groups internally.

**Practically: AWS handles everything related to nodes in EKS.**

---

# 5. Installing Helm

```bash
choco install kubernetes-helm
```

✔ Installs Helm on Windows using Chocolatey.

---

# 6. Helm Commands & Usage

### View your Helm repositories

```bash
helm search repo
```

### Add Bitnami Helm Repo

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```

### Search Helm Hub

```bash
helm search hub nginx
```

### Artifact Hub

Browse charts at:
**[https://artifacthub.io/packages/helm/](https://artifacthub.io/packages/helm/)**

### Download a Helm Chart

```bash
helm pull bitnami/nginx --untar=true
```

✔ Downloads + extracts the chart for customization.

---

# 7. Helm and Charts – Notes

* Helm is a **package manager** for Kubernetes.
* Helm packages are known as **Charts**.
* Charts help define, install, and upgrade complex K8s apps.
* Charts can be **versioned**, **shared**, and **published**.
* Helm Charts accept input parameters via **values.yaml**.
* Kustomize and Jinja-like templating is used behind the scenes.
* Many popular packages already available (Nginx, MariaDB, WordPress, Redis, etc.)

---

# 8. Helm Chart File Structure

A typical Helm chart looks like this:

```
mychart/
  Chart.yaml       # Metadata
  values.yaml      # Default input configs
  templates/       # Kubernetes manifests
      service.yaml
      deployment.yaml
```

---


You can now directly paste this into your GitHub README.
